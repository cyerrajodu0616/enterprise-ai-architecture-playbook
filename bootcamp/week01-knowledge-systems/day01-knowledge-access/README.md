# Day 1 — How Should an AI System Access Enterprise Knowledge?

## Status

Lesson reasoning completed. Final access architecture remains conditional on unresolved HealthSure requirements.

## Executive Summary

HealthSure wants an internal service-agent assistant that can answer policy questions and show governing evidence.

The main architectural lesson is that we should not begin with RAG. We should begin by identifying the business question, the authoritative source for each required fact, version/freshness/correctness requirements, and the simplest mechanism that satisfies them.

No final long-context-versus-retrieval decision was accepted.


## Architecture Reasoning Journal

This section preserves the important reasoning path from the Day 1 discussion. It is intentionally not a raw transcript. It records the learner's actual architectural thinking, the challenge applied, and the refined principle.

### Decision Point 1 — What should we ask before deciding whether to build RAG?

**Mentor scenario**

HealthSure wants an internal service-agent assistant. The initial temptation is to ask, “Should we build RAG?”

**Learner's initial reasoning**

The learner first asked:

- What information are we trying to serve?
- Is it general policy information such as deductible rules, coverage, or exclusions?
- Is it member-specific information such as remaining deductible or claim history?
- How many products exist?
- How frequently does policy information change?
- How much request volume exists?
- What latency is acceptable?
- What is the cost of a wrong answer?

For the simplest solution, the learner proposed:

- use text search before a vector database;
- use structured data when policy information is structured;
- combine structured data and text search when needed.

**Challenge**

The important missing dimension was authority.

Document size and corpus scale matter, but they do not answer:

- Which source is authoritative?
- Which policy version applies?
- Can all agents access the same data?
- Must the answer cite governing language?
- What is the consequence of stale or incorrect information?

The learner was also challenged on starting the simplicity ladder at “text search.” If the complete relevant document can be supplied directly, search itself may be unnecessary.

**Refined reasoning**

The access decision should begin with:

```text
What question is being asked?
    ↓
What facts are required?
    ↓
Where is each fact authoritative?
    ↓
What version/effective date applies?
    ↓
What freshness/correctness/latency is required?
    ↓
What is the simplest mechanism that satisfies that?
```

**Principle learned**

> Do not confuse “knowledge the AI needs” with “documents the AI must search.”

**What would change the position**

Real query patterns, corpus size, source authority, security boundaries, latency, and evaluation evidence.

---

### Decision Point 2 — Should all enterprise data go into the vector database?

**Mentor scenario**

Product proposes storing policy data, claims data, member accumulators, and related information in one vector database.

**Learner's initial reasoning**

The learner rejected the proposal because:

- there are structured and unstructured information types;
- policy information changes and would require vector-index updates;
- policy versions must be preserved;
- claims and accumulators change frequently;
- embeddings could become stale;
- data freshness must be clarified.

The learner also proposed converting some unstructured policy content into structured fields such as deductible, copay, and covered services.

**Challenge**

The learner was challenged on two points.

First, the core issue is not merely “structured vs unstructured.” It is:

- system of authority;
- freshness;
- version semantics;
- correctness;
- operational ownership.

Second, converting all policy language into structured data can create another derived source that must stay synchronized with the governing policy. Complex exclusions and conditions may lose meaning.

**Refined reasoning**

Keep changing operational facts in authoritative structured systems when possible. Use document retrieval only for knowledge that truly requires document interpretation.

For policy language:

- determine the applicable policy/version deterministically;
- constrain retrieval by metadata;
- then let the LLM interpret already-qualified evidence.

**Principle learned**

> Semantic similarity cannot establish authority, freshness, or legal/business applicability.

**What would change the position**

If source systems cannot support the workload, or a governed serving layer can satisfy freshness, reproducibility, and access requirements better than direct access.

---

### Decision Point 3 — What should the request flow look like?

**Mentor scenario**

Question:

> Why did a member pay a particular amount for yesterday's ambulance claim?

**Learner's initial reasoning**

The learner proposed:

1. directly access the claims system for claim details, member accumulators, and network status;
2. identify the policy version from the policy system;
3. use the LLM to identify policy and coverage language for ambulance service;
4. combine the information and generate the response.

**Challenge**

The claims system may not own all required facts. Accumulators and network status may belong to different authoritative systems.

The LLM should not decide which policy language is authoritative. The system should first resolve:

- member;
- claim;
- service date;
- policy;
- version;
- effective date;
- product/jurisdiction if relevant.

Only after that should retrieval or context assembly occur.

**Refined reasoning**

```text
Question
    ↓
Resolve business context
    ↓
Authoritative operational facts + authoritative policy scope
    ↓
Qualified evidence package
    ↓
LLM synthesis and explanation
```

**Principle learned**

> Deterministic systems establish facts and scope. The LLM explains those facts.

**What would change the position**

If no authoritative deterministic source exists and the business explicitly accepts model-based inference.

---

### Decision Point 4 — Should the LLM calculate financial responsibility?

**Mentor scenario**

The claim system shows a member responsibility amount, and policy language describes deductible and coinsurance rules.

**Learner's evolving reasoning**

The lesson established that the LLM should not normally reproduce authoritative adjudication mathematics when the business system already owns the result.

**Challenge**

An LLM may be able to calculate the amount correctly, but “can calculate” is not the same as “should own the calculation.”

**Refined reasoning**

Use the authoritative adjudication or calculation system to provide:

- deductible applied;
- coinsurance;
- member responsibility.

Use the LLM to explain the result in plain language.

**Principle learned**

> Use AI for interpretation where uncertainty exists. Use deterministic computation where deterministic rules exist.

**What would change the position**

Only when no authoritative calculation system exists and the business formally accepts a model-based calculation path with appropriate validation and controls.

---

### Decision Point 5 — Why not copy everything into one centralized AI database nightly?

**Mentor scenario**

The CTO proposes copying claims, benefits, network, and policy data into a central AI database every night.

**Learner's initial reasoning**

The learner argued:

- direct integrations preserve fresher information;
- another system creates storage, compute, embedding, vector, and operational cost;
- traceability becomes harder because answers may need to be reconciled against source systems;
- nightly refresh cannot satisfy tighter freshness requirements.

**Challenge**

The centralized design is not inherently wrong.

It may be better when:

- source systems are fragile;
- request traffic is high;
- direct fan-out creates latency;
- bounded staleness is acceptable;
- reproducibility requires governed historical snapshots.

**Refined reasoning**

Reject centralization only when it fails the business requirements. Accept it when its simplification and operational isolation outweigh duplicated-state and staleness costs.

**Principle learned**

> Centralization is a trade-off, not a virtue or a defect.

**What would change the position**

Source-system capacity, traffic, freshness tolerance, reproducibility requirements, and latency evidence.

---

### Decision Point 6 — Should we cache accumulator data to hit a 2-second SLA?

**Mentor scenario**

The authoritative accumulator system sometimes takes 3–4 seconds, while Product asks for responses under 2 seconds.

**Learner's initial reasoning**

Before recommending anything, the learner asked:

- What is the business risk of 3–4 second latency?
- Are we losing customers or productivity because of that latency?
- Can users tolerate one extra second for a correct response?
- If the business impact is real, perhaps cache accumulator data.

**Challenge**

Caching cannot be evaluated without a freshness requirement.

A cache may make the answer faster but wrong if the accumulator changes after the cached value was written.

The use of “edge” was also challenged: if the delay is in the backend calculation, geographic proximity does not solve it.

**Refined reasoning**

First challenge and measure the latency requirement. Then define acceptable staleness. Only then evaluate cache, replica, or serving-store patterns.

**Principle learned**

> Do not optimize latency without defining the maximum acceptable staleness of the fact being accelerated.

**What would change the position**

Measured workflow impact, data-change frequency, source latency profile, and business tolerance for stale values.

---

### Decision Point 7 — Should we use a read replica?

**Mentor scenario**

The learner was given a hypothetical system in which the primary database had high p95 latency because of write contention, while a read replica responded much faster with bounded replication lag.

**Learner's initial reasoning**

The learner first proposed investigating:

- network latency;
- expensive operations;
- avoidable processing;
- whether read/write contention is causing the delay.

If the same system is overloaded by reads and writes, the learner proposed a read replica.

**Challenge**

The read replica does not automatically solve the requirement. Replication lag creates a freshness dimension that must be compared against a freshness SLA.

**Refined reasoning**

A read replica is justified only if:

```text
replica response latency <= latency requirement
AND
replication lag <= freshness requirement
```

**Principle learned**

> Read replicas trade freshness for serving performance and workload isolation.

**What would change the position**

If the bottleneck is not on the database read path, or if replication lag exceeds business tolerance.

---

### Decision Point 8 — Comparing primary latency with replica lag

**Mentor scenario**

Hypothetical values:

- primary p99 response latency: 6.1 seconds;
- replica p99 replication lag: 11 seconds.

**Learner's initial reasoning**

The learner initially reasoned that because the primary's p99 latency was lower than the replica's p99 lag, the primary might be preferable because it remains authoritative and avoids another system.

**Challenge**

This comparison mixes two different dimensions:

- 6.1 seconds is response latency;
- 11 seconds is data staleness.

They cannot be compared directly.

**Refined reasoning**

Compare each metric to its own requirement:

```text
Primary p95 response latency
    ↔ latency SLA

Replica p95 response latency
    ↔ latency SLA

Replica replication lag
    ↔ freshness SLA
```

Under the hypothetical example used in the lesson:

- replica response latency met the <2 second target;
- replica lag remained inside a 30-second staleness tolerance.

Therefore the replica could be justified despite being less fresh than the primary.

**Principle learned**

> Never compare metrics merely because they are both measured in seconds. Compare each metric to the business property it represents.

**What would change the position**

Different latency or freshness requirements, or evidence that the source can meet both directly.

---

### Decision Point 9 — What happens when replica lag exceeds the SLA?

**Mentor scenario**

Replica lag occasionally rises above the allowed freshness threshold.

**Learner's initial reasoning**

The learner proposed giving the agent a choice:

- return the answer immediately with an “as of” timestamp;
- or wait longer for the authoritative source.

The learner also noticed that if traffic is heavy, many fallback requests could increase waiting time further.

The learner proposed investigating where the lag is introduced before redesigning the system.

**Challenge**

Human choice is not appropriate for every risk class. A user should not always be asked to choose between speed and correctness.

Also, automatic fallback to the primary may create a feedback loop that worsens the outage.

**Refined reasoning**

Use risk-based degradation:

```text
Replica fresh enough?
    ├─ yes → serve replica
    └─ no
         ↓
    Is stale data safe?
         ├─ yes → serve with explicit timestamp
         └─ no  → authoritative source or abstain
```

Fallback must also consider:

- rate limits;
- concurrency;
- circuit breakers;
- retry policy;
- source protection.

**Principle learned**

> Graceful degradation is a business-risk policy, not merely a technical fallback rule.

**What would change the position**

Question risk class, freshness requirement, source-system capacity, and user workflow expectations.

---

### Day 1 Reasoning Summary

The learner's reasoning evolved from:

```text
Should we use text search or vectors?
```

to:

```text
What business question is being answered?
    ↓
What facts are required?
    ↓
Where is each fact authoritative?
    ↓
What version and effective date apply?
    ↓
How fresh must each fact be?
    ↓
What latency and correctness are required?
    ↓
What is the simplest access path?
    ↓
What failure and fallback behavior is acceptable?
```

That shift is the primary architectural outcome of Day 1.


## Formal Architecture Position

### Simplest solution ladder

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
6. Reranking / advanced retrieval
```

Different question classes may use different mechanisms permanently.

### Current recommendation

> Under the current known HealthSure state, do not select one universal knowledge-access architecture. Classify questions, identify authoritative sources, define freshness, latency, versioning, authorization, and correctness requirements, then choose the simplest access mechanism per information class.

### Strongest counterargument

A unified serving/retrieval platform may simplify integration, isolate fragile source systems, and improve reproducibility. It should be adopted if evidence shows those benefits outweigh staleness, duplication, lineage, and operating cost.

### Decision state

- Vector database as universal canonical store: not recommended.
- Structured operational facts via authoritative systems: current preferred direction.
- Policy language via direct context/search/retrieval: conditional.
- Central serving layer/read replica/cache: conditional on measured need.
- Long context vs retrieval: deferred to Day 2.

## Cost and Operations

Relevant cost drivers:

- integrations;
- serving copies;
- indexing and embeddings;
- model usage;
- observability;
- reconciliation;
- source protection;
- on-call ownership.

Operational design must consider:

- source latency;
- source capacity;
- replication lag;
- retry behavior;
- fallback storms;
- degradation;
- lineage;
- “as of” timestamps.

## Security and Governance

The design must eventually establish:

- identity and authorization boundaries;
- member-data isolation;
- policy-version lineage;
- source attribution;
- audit evidence;
- safe prompt/log handling.

## Evaluation Evidence Needed

- real agent question taxonomy;
- corpus inventory;
- source-of-truth map;
- policy version semantics;
- freshness requirements;
- latency requirements;
- business impact of latency;
- source capacity;
- authorization model;
- evaluation dataset and thresholds.

## What Would Change the Recommendation?

- corpus growth;
- traffic growth;
- source-system constraints;
- latency evidence;
- authorization complexity;
- retrieval-quality evidence;
- model/context economics;
- new business use cases.

## Next Lesson

Day 2 — Long Context vs Retrieval

> When does retrieval provide enough value to justify an additional subsystem compared with supplying the relevant context directly?
