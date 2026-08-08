# Day 1 — How Should an AI System Access Enterprise Knowledge?

## Status

Reasoning developed. Architecture decision deferred pending evidence.

## Architectural Question

> What is the simplest knowledge-access architecture that satisfies the HealthSure business requirement?

## Business Context

HealthSure's initial use case is an internal policy knowledge assistant for service agents. The assistant is advisory and should help agents find governing policy language and understand where an answer came from.

Day 1 deliberately does **not** assume RAG, embeddings, a vector database, or a centralized AI data store.

## Core Learning

The architecture problem is not "How do we build RAG?"

The architecture problem is:

```text
Business question
    ↓
What facts are required?
    ↓
Where is each fact authoritative?
    ↓
How fresh must each fact be?
    ↓
How risky is incorrect or stale information?
    ↓
What latency does the business actually require?
    ↓
What is the simplest access mechanism?
    ↓
Direct context / SQL / API / search / retrieval
    ↓
LLM synthesis where appropriate
```

A vector database solves a retrieval problem. It is not automatically the architecture for AI access to enterprise data.

## Questions to Ask Before Choosing an Access Pattern

### Business and information need

- What information do internal service agents need?
- Are questions about general policy rules such as coverage, exclusions, deductible rules, and copays?
- Are questions about member-specific operational state such as remaining deductible, claim status, or prior claims?
- Can a single question require both policy knowledge and member-specific state?
- What measurable business outcome should improve?
- What is the consequence of a wrong answer?

### Authority and correctness

- Which system or document is authoritative for each fact?
- If two sources disagree, which source wins?
- Which policy version applies to a member or claim?
- Do effective date, service date, enrollment date, state, product, or plan affect the answer?
- Must the assistant cite exact governing policy language?
- When should the assistant abstain rather than answer?

### Scale, workload, security, and freshness

- How many products and policy variants exist?
- How large and well structured are the documents?
- How frequently do policies change?
- How many requests are expected?
- What are the p50, p95, and p99 latency expectations?
- Can all agents access the same policy, member, and claim data?
- Where must authorization be enforced?
- What freshness is required for each fact type?

Freshness is a property of a fact, not one global number for the whole AI system.

Hypothetical learning example — not a HealthSure fact:

```text
Policy document       → hours/day may be acceptable
Claim status          → minutes may be required
Deductible balance    → seconds may be required
```

## Important Architectural Boundary: Knowledge Type

Two questions can sound similar to a user but require different access mechanisms.

```text
"What does this policy cover?"
        ↓
Policy / plan language
        ↓
Direct context or search/retrieval may be appropriate

"How much deductible does this member have left?"
        ↓
Member-specific changing state
        ↓
Authoritative API / database access is usually more appropriate
```

A combined question may require both mechanisms.

## Simplest-Solution Ladder

```text
1. Direct / supplied context
        ↓ insufficient?
2. Structured SQL or API access
        ↓ insufficient?
3. Traditional lexical search
        ↓ insufficient?
4. Semantic retrieval
        ↓ insufficient?
5. Hybrid retrieval
        ↓ insufficient?
6. More sophisticated retrieval / reranking
```

This is not necessarily a migration sequence. Different question classes may permanently use different mechanisms.

## Why Not Put Everything in a Vector Database?

The lesson rejected using one vector database as the canonical store for all policy, claim, member, and accumulator information under the explored assumptions.

Reasons:

- structured and unstructured information have different access semantics;
- live operational facts may become stale when copied into indexes;
- policy correctness depends on version and effective-date semantics, not only semantic similarity;
- another copy creates reconciliation, lineage, monitoring, storage, compute, and operational obligations;
- embeddings do not make stale data current;
- a vector store is not automatically the authoritative system of record.

## Policy Documents: Do Not Over-Structure Automatically

Converting policy language into structured fields such as service type, deductible, copay, and coverage rules can help for stable deterministic rules, but it should not be automatic.

A structured representation becomes another derived source that must remain synchronized with governing policy language. Complex exclusions, exceptions, and conditional clauses may lose meaning during normalization.

## Example Request Flow

Hypothetical question:

> Why did a member pay a particular amount for an ambulance claim?

```text
Agent question
    ↓
Resolve member + claim + service date
    ↓
Operational facts                         Governing policy language
- claim details                           - identify applicable policy
- benefit accumulators                    - resolve effective version
- network/provider state                  - retrieve within qualified scope
    ↓                                          ↓
Authoritative operational systems          Authoritative policy source
                 \                         /
                  \                       /
                    Evidence package
                          ↓
                         LLM
                          ↓
                 Explanation + citations
```

Principle:

> Deterministic systems establish facts and scope. The LLM explains and synthesizes those facts.

The LLM should not decide which policy is authoritative when a deterministic system can establish that boundary.

## Deterministic Calculation vs LLM Reasoning

For financial or adjudication-style calculations, the preferred boundary is normally:

```text
Authoritative calculation / adjudication logic
        ↓
Calculated values
        ↓
LLM explanation
```

Do not use the LLM to reproduce deterministic calculations when an authoritative rule engine or source system already owns the result.

## Centralized AI Serving Copy vs Direct Source Access

A centralized serving layer may be attractive when source systems are fragile or rate-limited, cross-system request latency is too high, bounded staleness is acceptable, traffic is high, or reproducibility requires governed historical snapshots.

It may be inappropriate when freshness requirements are tighter than replication can guarantee, source systems can already serve the workload safely, duplicated state creates more reconciliation risk than value, or the main justification is simply "one place for AI data."

## Latency: Diagnose Before Adding Architecture

Before adding caching, replicas, or serving stores, determine where latency is introduced.

```text
Request latency
    ├─ network?
    ├─ slow query?
    ├─ expensive aggregation?
    ├─ write contention?
    ├─ downstream dependency?
    └─ application / serialization overhead?
```

A read replica only helps when the dominant latency is related to the read path it can improve.

## Read Replica Reasoning

Hypothetical learning scenario — not a HealthSure fact:

```text
Primary database
p50 = 700 ms
p95 = 3.4 sec
p99 = 6.1 sec

Cause: heavy write contention

Read replica prototype
p50 = 120 ms
p95 = 350 ms
replication lag p95 = 4 sec
replication lag p99 = 11 sec

Business tolerance
response p95 < 2 sec
maximum staleness = 30 sec
```

Response latency and replication lag are different dimensions and should not be compared directly.

Under the hypothetical requirements above, the replica satisfies both latency and bounded-staleness targets, so it can be justified despite adding another operational component. The primary remains the system of record; the replica is a serving path.

## Degradation When Replica Freshness Fails

Hypothetical policy:

```text
Is replica freshness within SLA?
    │
    ├─ yes → use replica
    │
    └─ no
         ↓
    What is the business risk?
         │
         ├─ low risk → possibly return bounded-stale data with explicit timestamp
         │
         └─ high risk → use authoritative source or abstain
```

A human-choice option can be useful in some advisory workflows: provide the value immediately with an explicit "as of" timestamp, or offer a slower authoritative lookup. High-risk decisions should not always delegate correctness choices to the user.

## Fallback Risk

```text
Replica unhealthy
    ↓
More traffic hits primary
    ↓
Primary load increases
    ↓
Latency increases
    ↓
Retries / queues increase
    ↓
Potential cascading failure
```

Fallback policies must consider rate limits, concurrency controls, timeouts, retries, circuit breaking, and graceful degradation.

## Architectural Position at End of Day 1

### Known

- HealthSure's initial use case is an internal policy knowledge assistant.
- The assistant is advisory.
- It should ground policy answers in governing language.
- No production AI platform exists yet.

### Current reasoning position

- Do not assume RAG or a vector database.
- Classify questions by information type and authority first.
- Prefer authoritative SQL/API access for changing structured operational facts.
- Use direct context, lexical search, semantic retrieval, or hybrid retrieval for policy language only when each mechanism earns its complexity.
- Treat policy version and effective-date semantics as correctness boundaries.
- Diagnose latency before introducing caches, replicas, or serving stores.
- Replicas and caches are acceptable only when bounded staleness and business risk permit them.
- Preserve timestamps and lineage when serving replicated or cached information.

### Assumptions

No new HealthSure scale, latency, freshness, or product-count assumptions were accepted as facts during this lesson. Numerical examples were hypothetical scenarios for architectural analysis.

### Open Questions

- What question categories do agents actually ask?
- Which facts are policy knowledge versus member-specific operational state?
- Which systems are authoritative for policy, claims, benefits accumulators, provider/network state, and enrollment?
- Which policy-version semantics determine the governing document?
- What freshness is required per fact type?
- What response latency is required per question class?
- What is the measured business impact of additional latency?
- What is the consequence of stale or incorrect data per question class?
- What authorization rules apply to policy and member data?
- What query/load limits do operational systems have?
- How large and well-structured is the policy corpus?
- How will answer and retrieval quality be evaluated?

## Decision Status

No ADR is accepted from Day 1. The important decision is intentionally deferred until HealthSure requirements and evidence are sufficient.

## Next Topic

Day 2 — Long Context vs Retrieval

> When does retrieval provide enough value to justify an additional subsystem compared with supplying the necessary context directly to the model?
