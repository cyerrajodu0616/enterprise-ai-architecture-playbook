# Planner Prompt — Generate Week 1 Day 2 Long Context vs Retrieval Artifacts

## Feature context

The Week 1 Day 2 mentoring session is complete, but the repository contains only Day 1 artifacts. The lesson developed a conditional recommendation to run a controlled HealthSure pilot using qualified long context while keeping retrieval deferred until measured quality, latency, context-capacity, authorization, or total-cost evidence justifies the additional subsystem.

This is not an accepted production architecture. Numerical workload, latency, cost, accuracy, and pilot results discussed in the lesson were hypothetical learning scenarios, not HealthSure facts. The ADR must therefore be **Experimental** and all hypothetical values must be labeled.

Preserve the learner's actual reasoning evolution rather than rewriting it into a perfect final answer. The main shifts were:

1. Long context was initially selected because a single product document could fit, then refined to qualified long context because fitting a context window does not establish correctness, latency, citation quality, or economics.
2. Deterministic policy applicability, precedence, version, effective date, product, jurisdiction, and authorization must be resolved before documents reach the model.
3. An amendment may modify rather than replace the base policy; context assembly must preserve the complete applicable policy package.
4. Historical versions are not globally stale: they are applicable or inapplicable for a specific business event date. Inapplicable versions must not cross the model boundary.
5. Evaluation must separate resolver failure, context omission, model interpretation failure, citation failure, and safe abstention. Average accuracy alone is insufficient.
6. A controlled pilot was preferred because the decision is reversible and can convert assumptions into evidence.
7. Accepted-unchanged answers are not proven-correct answers. Business outcomes, independent review, correction effort, abandonment causes, safety incidents, latency, and cost per successful answer must be measured.
8. Claim context should be immutable and explicit. Ambiguous or missing service dates require qualification; date defaults are permitted only for validated, allowlisted date-independent categories.
9. Audit logs, request registries, and caches have different responsibilities. Do not reuse a completed claim answer after an authoritative input changes.
10. Do not add stable-component caching until recomputation creates a measured business problem.

## Governing constraints

- Follow `ARCHITECTURE_MANIFESTO.md`, `PLAYBOOK_GUIDELINES.md`, `ROADMAP.md`, and the Week 1 sequence.
- Do not invent HealthSure facts, requirements, regulations, prices, traffic, latency targets, corpus dimensions, accuracy results, or operational capacity.
- Explicitly label all lesson numbers such as 40 documents, 5,000 requests/month, 8-second p95, 85,000 tokens, evaluation percentages, and pilot counts as hypothetical learning scenarios.
- State the recommendation conditionally and include its strongest counterargument and change triggers.
- Keep HealthSure's assistant advisory. Do not imply it makes authoritative coverage decisions or executes business actions.
- Use the Day 1 artifacts as formatting and navigation precedents.
- Use the date `2026-08-08` for the lesson and ADR review metadata.
- Do not add a retrieval/vector/reranking component to HealthSure's accepted current state.

## Files to create

### 1. `bootcamp/week01-knowledge-systems/day02-long-context-vs-retrieval/README.md`

Create the canonical detailed Markdown lesson. It must include all 19 lesson sections required by `PLAYBOOK_GUIDELINES.md:20-274` and a curated Architecture Reasoning Journal using the required six-field structure from `PLAYBOOK_GUIDELINES.md:498-550`.

Include at least these representative decision points:

- why qualified long context is simpler than retrieval for the bounded pilot;
- why "fits in context" is insufficient evidence;
- deterministic document/package selection and amendment precedence;
- historical-version contamination as a release blocker;
- distinguishing missing evidence from evidence the model overlooked;
- why raw accuracy cannot decide between architectures;
- pilot choice, missing business outcomes, and safety investigation;
- date applicability, conservative classification, and claim-scoped context;
- background requests, audit versus cache, and cache invalidation;
- why stable-component reuse is deferred.

End with the conditional recommendation:

> Under the current assumptions and constraints, HealthSure should begin with qualified long context in a controlled agent pilot because the applicable policy package can be selected deterministically, fits within usable model context, has satisfied the current hypothetical evaluation evidence, and avoids introducing retrieval infrastructure before it demonstrates necessary value. We accept higher model-input cost, hypothetical eight-second p95 latency, and possible long-context attention limitations to gain simpler operations, complete qualified policy context, lower retrieval-omission risk, and faster pilot delivery. We would introduce retrieval only when measured quality, latency, context-capacity, authorization, or total-cost evidence shows a material net improvement after retrieval's full operating obligations are included.

State that Day 3 is `Document Ingestion, Structure, Versions & Metadata`.

### 2. `bootcamp/week01-knowledge-systems/day02-long-context-vs-retrieval/index.html`

Create a responsive standalone HTML rendering of the complete lesson, following the visual and structural precedent in `bootcamp/week01-knowledge-systems/day01-knowledge-access/index.html`. It must contain the Architecture Reasoning Journal, alternatives, counterargument, cost, operations, security/governance, evaluation, recommendation, change triggers, and navigation to Day 1 and Day 3. Do not use external runtime dependencies.

### 3. `bootcamp/week01-knowledge-systems/day02-long-context-vs-retrieval/architects-challenge.md`

Create a realistic challenge in which HealthSure's qualified long-context pilot expands across policy packages with versions, amendments, benefit schedules, authorization boundaries, and uncertain cost/latency evidence. Require the learner to state assumptions, recommendation, alternatives, counterargument, cost, operations, security/governance, evaluation, and change triggers. Do not supply a single correct answer.

### 4. `adr/0002-qualified-long-context-pilot.md`

Create ADR-0002 with every field required by `PLAYBOOK_GUIDELINES.md:290-313`.

- Title: `Qualified Long Context Pilot for HealthSure Policy Knowledge`
- Status: `Experimental`
- Date: `2026-08-08`
- Owners: `HealthSure policy knowledge product, architecture, policy-domain, security, and operations owners (ownership assignments remain to be confirmed)`
- Decision: run a bounded, advisory qualified-long-context pilot; deterministic applicability/package validation precedes the model; retrieval remains deferred.
- Explain why Experimental rather than Accepted.
- Explicitly state no production component is accepted and no real pilot has occurred.
- Strongest counterargument: retrieval may materially reduce context size and latency and improve attention at sufficient scale, but adds omission, version-filtering, ingestion, indexing, evaluation, and on-call obligations.
- Review triggers: measured business outcomes, material answer errors, historical-version contamination, package completeness, context capacity, latency, cost per successful answer, corpus/product growth, authorization granularity, and retrieval comparison evidence.
- Relationship: refines but does not supersede ADR-0001.

### 5. `cheatsheets/week01-day02-long-context-vs-retrieval.md`

Follow the cheat-sheet requirements at `PLAYBOOK_GUIDELINES.md:339-353`. Include the simplicity rule, applicability boundary, failure taxonomy, total-cost comparison, pilot gates, strongest counterargument, interview framing, and change triggers.

### 6. `case-studies/healthsure/day02-long-context-vs-retrieval-update.md`

Record the new business requirement, previous position, Experimental decision, no accepted production components, risks, metrics, open questions, and Day 3 pressure. Clearly distinguish known HealthSure state from hypothetical lesson scenarios.

### 7. `architect-journal/week01-day02-reflection.md`

Answer all five reflection questions from `PLAYBOOK_GUIDELINES.md:266-274` in the learner's demonstrated voice. Capture the surprise that fitting content is not sufficient, the challenge to cost-only selection, remaining discomfort about historical applicability and model omission, evidence required before production, and the simpler-pilot conclusion.

## Existing files to update

### 8. `CURRENT_SESSION.md`

Replace lines 3-33 in full.

Before:

```markdown
## Program Position

- Week: 1
- Day: 1 completed
- Next: Day 2 — Long Context vs Retrieval

## Reasoning Shifts

- Moved from “search vs vector DB” to “fact → authority → freshness → risk → access mechanism.”
- Challenged the assumption that all AI data belongs in one vector store.
- Corrected the comparison between response latency and replication lag.
- Established that stale-fast vs authoritative-slow behavior must be governed by business risk.
- Identified fallback storms as an operational risk.

## Decisions

No final long-context-vs-retrieval decision accepted.

## Artifacts

- detailed HTML lesson with Architecture Reasoning Journal;
- Proposed ADR;
- cheat sheet;
- HealthSure update;
- Architect's Challenge;
- open questions;
- Architect's Reflection.

## Next Topic

Day 2 — Long Context vs Retrieval
```

After:

```markdown
## Program Position

- Week: 1
- Day: 2 completed
- Next: Day 3 — Document Ingestion, Structure, Versions & Metadata

## Reasoning Shifts

- Moved from “the document fits, so use long context” to “qualified context must also satisfy applicability, completeness, quality, latency, cost, and authorization.”
- Established deterministic policy applicability and package validation as pre-model correctness boundaries.
- Distinguished inapplicable historical-version contamination from model interpretation and context-assembly failures.
- Rejected raw accuracy and unchanged-agent acceptance as sufficient evidence of correctness or business value.
- Chose a controlled, reversible pilot to convert assumptions into evidence.
- Deferred retrieval, response caching, and stable-component reuse until measured limitations justify their additional obligations.

## Decisions

- ADR-0002 records an Experimental qualified-long-context pilot; no production architecture is accepted.
- Retrieval remains deferred pending workload-relevant quality, latency, context-capacity, authorization, and total-cost evidence.
- Materially wrong answers, inapplicable policy versions, and silent package incompleteness are pilot release blockers.
- Ambiguous date applicability must fail toward qualification or abstention rather than inference.

## Assumptions and Open Evidence

- Corpus size and structure, production traffic, latency requirements, cost, authorization boundaries, policy-version semantics, and evaluation thresholds remain unverified HealthSure evidence.
- All numerical workloads, latency, token, accuracy, and pilot examples used during Day 2 were hypothetical learning scenarios.

## Artifacts

- detailed Markdown and HTML lesson with Architecture Reasoning Journal;
- Experimental ADR-0002;
- five-minute cheat sheet;
- HealthSure Day 2 update;
- Architect's Challenge;
- updated open questions;
- Architect's Reflection.

## Next Topic

Day 3 — Document Ingestion, Structure, Versions & Metadata
```

### 9. `bootcamp/week01-knowledge-systems/README.md`

Change line 3.

Before:

```markdown
**Status:** Not Started
```

After:

```markdown
**Status:** In Progress — Days 1 and 2 completed; next is Day 3
```

Replace line 55.

Before:

```markdown
### Day 2 — Long Context vs Retrieval
```

After:

```markdown
### [Day 2 — Long Context vs Retrieval](day02-long-context-vs-retrieval/README.md)
```

Preserve the existing purpose, exploration topics, question, and counterargument at lines 57-75.

### 10. `adr/index.md`

Append after line 5:

```markdown
| [ADR-0002](0002-qualified-long-context-pilot.md) | Qualified Long Context Pilot for HealthSure Policy Knowledge | Experimental | [Week 1 Day 2](../bootcamp/week01-knowledge-systems/day02-long-context-vs-retrieval/README.md) | 2026-08-08 |
```

### 11. `adr/0001-enterprise-knowledge-access-pattern.md`

Replace lines 16-18.

Before:

```markdown
## Why Proposed, Not Accepted

Long context vs retrieval, centralized serving, and replica/cache choices still depend on unresolved HealthSure evidence.
```

After:

```markdown
## Why Proposed, Not Accepted

ADR-0002 records a qualified-long-context pilot as Experimental, but production acceptance, centralized serving, and replica/cache choices still depend on unresolved HealthSure evidence.
```

Append to the Review Triggers section:

```markdown

## Related Decisions

- ADR-0002 refines the policy-language access path experimentally and does not supersede this proposed question- and source-aware pattern.
```

### 12. `case-studies/healthsure/current-state.md`

Replace lines 32-44.

Before:

```markdown
## Immediate Architectural Work

- Classify the questions service agents actually ask.
- Identify the authoritative source for every required fact.
- Confirm policy version and effective-date semantics.
- Define freshness requirements per fact type.
- Define response-latency requirements per question class.
- Determine consequences of stale or incorrect answers.
- Confirm authorization boundaries.
- Measure source-system capacity and query limitations.
- Measure policy corpus size and structure.
- Compare direct context, lexical search, semantic retrieval, hybrid retrieval, and structured access.
- Define evaluation evidence and acceptance criteria.
```

After:

```markdown
## Day 2 Reasoning Update

No production architecture component has been accepted.

ADR-0002 records a qualified-long-context pilot as Experimental. Before any policy material reaches the model, deterministic logic must establish product, jurisdiction, service date, applicable version and amendments, package completeness, and authorization. Retrieval remains deferred until measured limitations of the simpler design justify its additional ingestion, indexing, filtering, evaluation, and operating obligations.

Materially wrong answers, inapplicable historical versions, and silently incomplete policy packages are pilot release blockers. Safe abstention and manual review may be acceptable only under business-approved thresholds and operating capacity.

All numerical corpus, traffic, token, latency, evaluation, and pilot examples discussed during Day 2 were hypothetical learning scenarios, not HealthSure facts. Real business outcomes, corpus structure, policy-version semantics, authorization boundaries, latency requirements, total cost, and evaluation thresholds remain open evidence.

## Immediate Architectural Work

- Classify the questions service agents actually ask.
- Identify the authoritative source for every required fact.
- Confirm policy version, amendment-precedence, effective-date, and package-manifest semantics.
- Define freshness and response-latency requirements per question class.
- Determine consequences of stale, incomplete, historically inapplicable, or incorrect information.
- Confirm authorization boundaries and claim-context binding.
- Measure source-system capacity, policy corpus size, structure, and change patterns.
- Define golden, held-out, adversarial, and pilot evaluation evidence and acceptance criteria.
- Measure qualified-long-context research-time savings, handle-time effect, correction effort, abstention/manual-review burden, latency, and cost per successful answer.
- Compare retrieval only after measured quality, latency, context-capacity, authorization, corpus-growth, or total-cost limits justify it.
```

### 13. `case-studies/healthsure/open-decisions.md`

Replace line 5.

Before:

```markdown
- Long context versus retrieval
```

After:

```markdown
- Long context versus retrieval — qualified long context is Experimental for a controlled pilot; production acceptance and retrieval migration triggers remain open
```

### 14. `open-questions/index.md`

Preserve HS-KA-001 through HS-KA-011. Append:

```markdown
| HS-LCR-001 | Can HealthSure deterministically resolve the applicable policy package by product, jurisdiction, service date, version, amendment, and authorization? | Required correctness boundary before either long context or retrieval. | Policy-domain rules and adversarial version tests available. | Open |
| HS-LCR-002 | Does qualified long context meet material-answer, limitation-recall, citation, abstention, and historical-version safety thresholds? | Experimental; no real HealthSure evaluation evidence exists yet. | Golden, held-out, adversarial, and pilot evaluations completed. | Open |
| HS-LCR-003 | What business outcome and end-to-end latency thresholds make the assistant valuable to agents? | Acceptance and latency are not sufficient without workflow outcomes. | Before/after research time, handle time, usefulness, and latency distributions measured. | Open |
| HS-LCR-004 | What is the cost per successful answer for qualified long context versus retrieval? | Compare complete TCO, not model tokens alone. | Real workload, token, retrieval, engineering, review, and incident costs measured. | Open |
| HS-LCR-005 | Which question categories may safely omit or default a service date? | Default to date-dependent unless a category is explicitly validated and allowlisted. | Policy-domain approval and regression evidence available. | Open |
| HS-LCR-006 | When would retrieval earn its additional ingestion, indexing, filtering, reranking, evaluation, and operational obligations? | Only after a measured long-context limit and a material net improvement are demonstrated. | Quality, latency, context-capacity, authorization, corpus-growth, or TCO threshold breached. | Open |
| HS-LCR-007 | Can in-flight deduplication reduce duplicate requests without unsafe completed-answer reuse? | Prefer immutable request identity; defer broad answer caching. | Pilot reuse, abandonment, notification, and wasted-cost measurements available. | Open |
```

## Verification

After implementation:

1. Run `git diff --check`.
2. Run `rg -n "5000|5,000|8-second|85,000|97%|96%|1,000|760|90|70|30|50"` across all new and updated Day 2 files and verify every numerical scenario is explicitly labeled hypothetical.
3. Run `rg -n "Accepted|accepted production|production architecture"` across Day 2 files and verify the decision is consistently Experimental and no production component is claimed as accepted.
4. Verify every link target added to Markdown or HTML exists.
5. Open/render `day02-long-context-vs-retrieval/index.html` and visually verify desktop and mobile readability, navigation, headings, tables, diagrams, and overflow.
6. Confirm `git status --short` includes only the files listed in this prompt and the prompt file itself.
7. Compare the Markdown lesson and HTML lesson section-by-section so neither loses the reasoning journal, cost, operations, security/governance, evaluation, recommendation, or change triggers.

## Gotchas

- Do not present the hypothetical evaluation tables or pilot counts as real HealthSure results.
- Do not state that a manifest repair is guaranteed to eliminate omissions; it is a hypothesis requiring regression and held-out validation.
- Do not equate accepted-unchanged answers with correctness.
- Do not call historical versions simply stale; applicability depends on the relevant business date.
- Do not treat an amendment as a full replacement unless verified policy semantics establish that relationship.
- Do not use an audit log as a response cache.
- Do not recommend caching completed claim answers across authoritative state changes.
- Do not silently infer a missing service date for date-dependent questions.
- Do not choose retrieval solely because token cost is higher; compare business outcomes and full TCO.
- Do not create or claim a final Week 1 architecture during Day 2.
