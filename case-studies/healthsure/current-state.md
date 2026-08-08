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
