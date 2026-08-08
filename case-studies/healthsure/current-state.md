# HealthSure Current State

## Day 0 Baseline

HealthSure does not yet operate a production enterprise AI platform.

Source systems and policy documents are distributed across business domains and repositories. Their authority, ownership, update patterns, access controls, and data-quality responsibilities must be confirmed before an AI design depends on them.

Architecture decisions are not finalized. The playbook begins with questions and evidence rather than assuming a model, retrieval system, data platform, workflow engine, or shared AI platform.

## Initial Use Case

The first use case is an internal policy knowledge assistant for service agents. It should help users find relevant governing policy language and understand where an answer came from. It remains advisory and does not execute business actions or replace authoritative systems and accountable human decisions.

## Day 1 Reasoning Update

No production architecture component has been accepted.

The case study now distinguishes four knowledge/access responsibilities:

1. **Governing policy language** — authoritative versioned policy sources.
2. **Member and claim operational state** — authoritative structured systems accessed through controlled SQL/API boundaries when appropriate.
3. **Deterministic calculations** — owned by authoritative business logic or adjudication systems rather than recreated by an LLM when an authoritative result exists.
4. **LLM synthesis/explanation** — combines qualified evidence into an advisory response with traceable support.

The current architectural position is to avoid assuming RAG, embeddings, a vector database, or a centralized AI store until the business and evidence justify them.

Policy correctness is expected to depend on authority and version semantics, not semantic similarity alone. Exact HealthSure version rules remain an open question.

Freshness, latency, and acceptable staleness must be defined per fact type. Numerical freshness and latency examples explored during Day 1 were hypothetical and are not HealthSure requirements.

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
