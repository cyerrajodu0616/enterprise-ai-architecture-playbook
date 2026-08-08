# Day 2 — Long Context vs Retrieval

## Status

Lesson reasoning completed. A qualified-long-context pilot is recorded as Experimental; no production architecture has been accepted.

## 1. Executive Summary

HealthSure's advisory policy assistant must provide service agents with governing policy language while preventing an inapplicable policy version, incomplete policy package, or unsupported model conclusion from becoming a customer-facing answer.

The architectural tension is whether to supply a complete, deterministically qualified policy package to a long-context model or add ingestion, indexing, filtering, retrieval, and reranking to select passages.

Under current assumptions, the simplest viable direction is a controlled qualified-long-context pilot. Retrieval remains a credible alternative if measured quality, latency, context capacity, authorization granularity, corpus growth, or total cost demonstrates that its additional subsystem earns its place.

The strongest counterargument is that long context may consume substantially more tokens, respond more slowly, and overlook evidence within a large package. Retrieval may focus the model and improve unit economics, but it creates a new evidence-omission and historical-version failure surface.

## 2. Business Problem

Service agents need timely, understandable answers grounded in the policy package governing a specific business event. The business outcome is not merely an answer: it is a correct, useful advisory response that reduces research effort without increasing correction work, manual review, customer harm, or compliance exposure.

Known HealthSure facts remain limited:

- the first use case is an internal advisory policy knowledge assistant;
- agents need governing policy language and source traceability;
- source authority, version semantics, authorization, workload, latency, corpus, and evaluation thresholds remain unconfirmed.

The following were hypothetical learning scenarios, not HealthSure requirements or measurements: 40 documents, 5,000 monthly questions, 8-second p95 latency, 85,000 model-input tokens, evaluation percentages, and pilot-result counts.

## 3. Current State and Trigger

Day 1 rejected a universal vector store and separated governing policy knowledge, operational state, deterministic calculation, and LLM explanation. Long context versus retrieval remained deferred.

Day 2 introduces a bounded decision: if an applicable policy package can be resolved deterministically and fits within usable context, does retrieval deliver enough additional value to justify another subsystem?

## 4. Naive Explanation

Long context sends the model the complete qualified evidence. Retrieval searches an indexed corpus and sends only selected evidence.

Long context can avoid retrieval omission but may cost more and make evidence harder for the model to notice. Retrieval can reduce context and latency but may omit the clause that changes the answer. Neither mechanism establishes which policy governs; that must happen before either one.

## 5. Requirements, Constraints, and Assumptions

### Requirements

- identify the governing policy package before model invocation;
- prevent unauthorized or inapplicable versions from reaching the model;
- preserve amendments, schedules, definitions, exclusions, and precedence;
- cite evidence and abstain when applicability or completeness cannot be established;
- measure correctness, usefulness, latency, and cost as separate properties;
- retain sufficient evidence for investigation and reproduction.

### Constraints

- the assistant is advisory and does not replace authoritative systems or accountable human decisions;
- real HealthSure policy semantics and operating thresholds are not yet available;
- added components create security, governance, evaluation, and on-call obligations.

### Assumptions

- **Assumption:** a bounded applicable policy package can fit in usable model context. Evidence: hypothetical lesson scenario only. Impact if wrong: long context may become infeasible or unreliable. Validation: corpus inventory and representative context measurement.
- **Assumption:** deterministic applicability and package completeness can be implemented. Evidence: architectural requirement, not verified HealthSure capability. Impact if wrong: either design can use incorrect evidence. Validation: policy-domain rules and adversarial tests.
- **Assumption:** a controlled pilot is reversible and operationally supportable. Evidence: hypothetical scenario only. Impact if wrong: the pilot may create unacceptable risk or burden. Validation: ownership and readiness review.

## 6. Simplest Viable Solution

```text
Authenticated agent and claim/question context
        ↓
Deterministic applicability resolution
        ↓
Complete authorized policy-package validation
        ↓
Qualified policy context
        ↓
Model explanation with citations or abstention
```

Begin with this qualified-long-context path for a bounded pilot. Do not add ingestion, embeddings, retrieval, or reranking until a measured limitation requires them.

## 7. Technical Deep Dive

### Applicability boundary

The resolver must establish product, jurisdiction, relevant business or service date, policy version, amendments, schedules, precedence, and authorization. Historical versions are not globally stale; they are applicable or inapplicable for a particular event.

```text
No policy material crosses the model boundary unless:
product matches
AND jurisdiction matches
AND event date falls within applicability
AND policy-package relationship is valid
AND package completeness is confirmed
AND authorization permits access
```

If a required date is missing, the system should request qualification or abstain. Defaults belong only to explicitly validated and allowlisted date-independent question categories. Uncertain classification takes the more restrictive path.

### Package assembly

An amendment may change one clause without replacing the base policy. A benefits schedule may supply cost-sharing detail absent from both. The assembler must preserve the complete governing meaning rather than assume that the most specific document is sufficient.

### Claim-scoped execution

Capture an immutable request snapshot. If an agent moves from Claim A to Claim B while a request is running, the Claim A result remains bound to Claim A and must not appear as Claim B's answer. Authorization is revalidated before display.

### Failure taxonomy

```text
Materially wrong answer
├── wrong policy package selected
├── required evidence absent from context
├── evidence present but model overlooked it
├── precedence interpreted incorrectly
└── unsupported conclusion generated
```

These failures have different owners and corrective actions. Resolver errors are not repaired by prompting. Missing evidence is not the same as evidence the model failed to use.

### Request identity and reuse

Audit stores, request registries, and caches are distinct responsibilities. In-flight deduplication may use immutable request identity. A completed claim answer must not be reused after an authoritative input changes. Stable policy evidence might eventually be reused, but a dedicated stability/invalidation layer is deferred until recomputation becomes a measured business problem.

## 8. Architecture Options

### Option A — Qualified long context

Supply the complete qualified policy package or complete qualified sections after deterministic validation.

- Strengths: simpler operating model; preserves surrounding meaning; avoids retrieval omission.
- Weaknesses: higher token consumption; potentially slower; model may overlook distant evidence.
- Cost: model input dominates variable cost; resolver, assembly, evaluation, and audit remain operational costs.
- Security: authorization and applicability must precede context assembly.
- Suitable when: scope is bounded, package fits, and measured quality and economics satisfy requirements.

### Option B — Filtered retrieval

Resolve applicability first, then retrieve only within the authorized governing package.

- Strengths: smaller model context; potentially lower latency and inference cost; focused evidence.
- Weaknesses: evidence may be silently omitted; ingestion, indexing, filtering, reranking, and reprocessing must be operated.
- Cost: lower possible model cost but higher fixed engineering, storage, evaluation, and on-call cost.
- Security: applicability and authorization filters remain hard boundaries, not relevance hints.
- Suitable when: measured long-context limitations outweigh the full retrieval burden.

### Option C — Qualified sections without a retrieval subsystem

Use deterministic document structure and domain rules to select complete governing sections.

- Strengths: may reduce tokens while avoiding semantic retrieval infrastructure.
- Weaknesses: section rules and document structures can become another fragile assembly mechanism.
- Suitable when: document structure is reliable and required evidence can be selected deterministically.

## 9. Counterarguments and Devil's Advocate

The strongest objection to long context is that avoiding retrieval infrastructure may merely transfer complexity into token cost, latency, model attention, and package assembly. A retrieval system could create a materially better business outcome if it reduces latency and cost while maintaining applicability and evidence coverage.

Operations would challenge large repeated requests and incident attribution. Security would challenge claim context, authorization, notification privacy, and logs. Finance would challenge cost per successful answer rather than cost per call. Governance would reject a design unable to reproduce the policy version and evidence used.

## 10. Trade-off Analysis

- Context completeness versus focused attention.
- Model-variable cost versus retrieval fixed and operational cost.
- Faster pilot delivery versus preparation for unproven growth.
- Agent convenience versus required qualification for date-sensitive questions.
- Safe abstention versus manual-review capacity and business usefulness.
- Reuse efficiency versus invalidation and stale-conclusion risk.

## 11. Cost Model

```text
Long-context monthly cost
≈ requests × average input tokens × effective token price
+ output generation
+ resolver/assembler operations
+ evaluation, review, observability, and incidents
```

```text
Retrieval monthly cost
≈ requests × retrieval-query cost
+ model cost for retrieved context
+ ingestion, indexing, storage, and reprocessing
+ engineering and on-call ownership
+ evaluation, review, observability, and incidents
```

The comparison metric is cost per correct, useful answer—not model cost alone. Traffic, follow-up turns, document reuse, update frequency, manual review, and failure cost affect the crossover.

## 12. Operational Concerns

- Owners: policy domain, resolver/package assembly, application/model, security, evaluation, and operations; exact HealthSure assignments remain open.
- Monitor: applicability failures, package completeness, material errors, abstentions, corrections, latency, token use, and cost per successful answer.
- Degrade: request missing information or abstain; never silently select a convenient policy version.
- Rollback: disable affected question classes or the pilot cohort.
- Investigate: preserve request inputs, resolver decision, package identity, context, model/configuration, answer, citations, and customer impact.
- Background work: bind results to immutable claim context; navigation alone does not prove cancellation intent.

## 13. Security, Privacy, and Governance

- authenticate and authorize before access and again before result display;
- use least privilege across policy, member, and claim data;
- prevent inapplicable and unauthorized versions from entering context or retrieval scope;
- disclose the claim/service-date scope safely without unnecessary identifiers;
- separate audit retention from response caching;
- govern prompts, model/configuration versions, policy lineage, and manual-review evidence;
- avoid sensitive content in background notifications and logs.

## 14. Evaluation and Evidence

Use a human-reviewed golden dataset plus held-out and adversarial sets. For every question record the applicable package, required clauses, expected conclusion, limitations, acceptable citations, and abstention conditions.

Measure:

- applicability and package-selection accuracy;
- required-clause coverage;
- material-answer correctness;
- exclusion and limitation recall;
- unsupported-claim and wrong-version rates;
- citation correctness;
- appropriate and inappropriate abstention;
- research time, handle-time effect, correction effort, and usefulness;
- p50, p95, and p99 end-to-end latency by meaningful question class;
- token consumption and cost per successful answer.

Accepted-unchanged answers are not independently proven correct. A finite evaluation can establish zero observed material errors, not a true zero production error rate.

Release blockers include materially wrong conclusions, inapplicable historical versions, silent package incompleteness, and unsupported definitive answers. Pilot acceptance thresholds and manual-review capacity remain open HealthSure decisions.

## 15. Final Recommendation

> Under the current assumptions and constraints, HealthSure should begin with qualified long context in a controlled agent pilot because the applicable policy package can be selected deterministically, fits within usable model context, has satisfied the current hypothetical evaluation evidence, and avoids introducing retrieval infrastructure before it demonstrates necessary value. We accept higher model-input cost, hypothetical eight-second p95 latency, and possible long-context attention limitations to gain simpler operations, complete qualified policy context, lower retrieval-omission risk, and faster pilot delivery. We would introduce retrieval only when measured quality, latency, context-capacity, authorization, or total-cost evidence shows a material net improvement after retrieval's full operating obligations are included.

This recommendation is Experimental. No real HealthSure pilot or production acceptance is claimed.

## 16. What Would Change the Recommendation?

- a materially wrong answer or inapplicable policy version;
- repeated package-completeness or model-attention failures;
- unacceptable workflow latency;
- context capacity or authorization granularity that long context cannot satisfy;
- corpus or product growth;
- unsustainable cost per successful answer;
- retrieval demonstrating a material net improvement after full TCO and risk are included.

## 17. HealthSure Case-Study Update

Day 2 records an Experimental qualified-long-context pilot direction. No production component is accepted. Deterministic applicability and complete package validation become pre-model boundaries. Retrieval remains deferred. The next pressure is to establish trustworthy document structure, versions, metadata, lineage, and completeness for Day 3.

## 18. Architect's Challenge

See [Architect's Challenge](architects-challenge.md): determine whether an expanding policy domain should remain on qualified long context, adopt deterministic section assembly, or introduce filtered retrieval.

## 19. Architect's Reflection

The lesson shifted the decision from “does it fit?” to “can we qualify, assemble, evaluate, operate, and economically justify it?” The simpler pilot is appropriate only while evidence supports its boundaries. See the [Day 2 reflection](../../../architect-journal/week01-day02-reflection.md).

## Architecture Reasoning Journal

### Decision Point 1 — Is fitting inside context sufficient?

**Mentor question:** What makes long context sufficient for the initial scenario?

**Learner's initial reasoning:** Most agents work with one product, the document fits, and avoiding retrieval removes another system. Traffic, latency, and token cost should be revisited.

**Challenge:** Fitting proves capacity, not correctness, citations, latency, attention, authorization, or economics.

**Refined reasoning:** Use qualified long context only if deterministic selection and workload-relevant evaluation show it satisfies the business requirement.

**Principle learned:** Context capacity is not an architecture acceptance criterion.

**Change trigger:** Measured quality, latency, cost, authorization, or corpus limits.

### Decision Point 2 — Which documents enter context?

**Mentor question:** Should an amendment replace the base policy in context?

**Learner's initial reasoning:** Use a deterministic layer to determine precedence and send the document containing the answer.

**Challenge:** An amendment may change one clause while the rest of the base policy and benefits schedule remain governing.

**Refined reasoning:** Deterministically establish applicability and precedence, then preserve the complete governing package or evidence proven complete.

**Principle learned:** Deterministic logic determines what governs; assembly must preserve the meaning of what governs.

**Change trigger:** Verified policy semantics demonstrating a narrower complete package.

### Decision Point 3 — What is the most dangerous retrieval failure?

**Mentor question:** How should historical-version answers be treated?

**Learner's initial reasoning:** Historical-version contamination is more concerning because it can misstate whether a member has benefits.

**Challenge:** Historical policies are not universally stale; they can govern historical service dates.

**Refined reasoning:** Applicability is event-specific. Inapplicable versions must be excluded before retrieval or long-context assembly.

**Principle learned:** Semantic relevance cannot establish temporal applicability.

**Change trigger:** Verified version and effective-date rules with adversarial evidence.

### Decision Point 4 — What does a “failure” mean?

**Mentor question:** Did overlooked or omitted clauses cause wrong answers, abstentions, or incomplete explanations?

**Learner's initial reasoning:** The severity cannot be judged without knowing whether incorrect information reached the agent or customer; detected missing evidence can route to manual review.

**Challenge:** Undetected incompleteness may look like a complete context and produce confident error.

**Refined reasoning:** Classify failures by boundary and business consequence, not raw count.

**Principle learned:** Average accuracy hides critical failure modes.

**Change trigger:** Business-approved severity, abstention, and release thresholds.

### Decision Point 5 — Pilot or build retrieval for growth?

**Mentor question:** Should HealthSure pilot the simpler design or prepare for anticipated scale?

**Learner's initial reasoning:** Launch a controlled pilot because the team can support assembly and production evidence is more useful than a golden set alone.

**Challenge:** Manual research and model latency were initially compared incorrectly, agent multitasking was assumed, and long context still has operational cost.

**Refined reasoning:** Pilot because the design is bounded and reversible; measure end-to-end business value, safety, latency, and unit economics before expansion.

**Principle learned:** Use constrained pilots to convert assumptions into evidence.

**Change trigger:** Business outcomes, safety reports, cost, or latency violating thresholds.

### Decision Point 6 — Does agent acceptance prove success?

**Mentor question:** Can accepted-unchanged answers establish correctness?

**Learner's initial reasoning:** No; research time, handle time, usefulness, latency distributions, edits, abandonment, unreviewed answers, and customer impact are missing.

**Challenge:** Zero observed errors does not prove zero underlying production risk.

**Refined reasoning:** Hold expansion until safety incidents, review backlog, business outcomes, and independent correctness are resolved.

**Principle learned:** Adoption measures behavior; evaluation establishes safe value.

**Change trigger:** Completed independent review and business outcome evidence.

### Decision Point 7 — Missing service date and claim context

**Mentor question:** Should the system infer a missing service date?

**Learner's initial reasoning:** Benefits depend on service date; allow defaults only for categories where the date cannot change the outcome.

**Challenge:** Category classification becomes a correctness boundary and apparent date independence can change.

**Refined reasoning:** Default to date-dependent; allowlist validated exceptions; use authoritative selected-claim context when unambiguous and disclose the scope.

**Principle learned:** Uncertainty crossing a material correctness boundary fails toward qualification.

**Change trigger:** Evidence that an allowlisted category becomes version-sensitive or an authoritative event supplies the date safely.

### Decision Point 8 — Background requests and context binding

**Mentor question:** Cancel Claim A when the agent navigates to Claim B?

**Learner's initial reasoning:** Keep the request if the agent will return; cancel it otherwise to save model cost.

**Challenge:** Future intent cannot be inferred from navigation, and cancellation may save little after inference begins.

**Refined reasoning:** Bind results immutably to Claim A, permit explicit cancellation, measure reuse and waste, and never display Claim A's answer as Claim B's.

**Principle learned:** Request lifecycle policy depends on explicit intent and measured economics.

**Change trigger:** Material unused inference cost and verified cancellation savings.

### Decision Point 9 — Audit, cache, and changed claim state

**Mentor question:** Reuse the same answer after claim state changes?

**Learner's initial reasoning:** Generate a new answer because the prior conclusion reflected the previous point-in-time state.

**Challenge:** Stable policy evidence might be reusable, but identifying stable components adds another invalidation responsibility.

**Refined reasoning:** Regenerate during the pilot; measure whether recomputation creates a business problem before adding component-level reuse.

**Principle learned:** Cache evidence only within its freshness boundary; do not reuse conclusions across authoritative state changes.

**Change trigger:** Repeated queries and measured token/latency savings exceeding implementation, operations, evaluation, and incident risk.

## Next Lesson

Day 3 — Document Ingestion, Structure, Versions & Metadata

> How do we ensure the model receives the correct version of complete, authoritative information?

---

**Navigation:** [Previous: Day 1](../day01-knowledge-access/README.md) · [Week 1 overview](../README.md) · Next: Day 3 — Document Ingestion, Structure, Versions & Metadata
