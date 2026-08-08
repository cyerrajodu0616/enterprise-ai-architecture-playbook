# Current Session

## Program Position

- Week: 1
- Day: 1 completed; Day 2 next
- Topic completed: How Should an AI System Access Enterprise Knowledge?
- Next topic: Long Context vs Retrieval
- Status: Day 1 reasoning consolidated; architecture decision deferred pending evidence

## What Was Learned

- Start from the business question and authoritative facts, not from RAG.
- Separate policy knowledge from member-specific operational state.
- Structured changing facts usually belong behind authoritative SQL/API access rather than embeddings.
- Policy retrieval must respect authority, version, effective date, and access boundaries.
- Direct context, lexical search, semantic retrieval, and hybrid retrieval should be selected only when they earn their complexity.
- Deterministic systems should establish facts, scope, and calculations; the LLM should synthesize and explain.
- Freshness is fact-specific, not one global AI requirement.
- Diagnose latency before adding caches, replicas, or serving stores.
- Response latency and replication lag are different dimensions.
- Fallback to a primary system can create cascading load and must be designed deliberately.

## Decisions Made

No final architecture decision was accepted.

Current position:
- Do not assume a vector database or RAG.
- Do not use a vector database as the canonical store for live structured operational state.
- Preserve authoritative source boundaries.
- Defer long-context-versus-retrieval until corpus, query, cost, latency, authorization, and quality evidence is available.

## Assumptions

No hypothetical lesson numbers were promoted to HealthSure facts.

## Open Questions

- Agent question categories and frequencies
- Authoritative source per fact
- Policy version/effective-date semantics
- Freshness per fact type
- Latency per question class
- Consequence of stale or incorrect answers
- Authorization boundaries
- Source-system capacity and query limits
- Policy corpus size and structure
- Evaluation methodology and acceptance criteria

## HealthSure Changes

No production component was added.

The case-study model now explicitly distinguishes:
- governing policy knowledge;
- member/claim operational state;
- deterministic calculations;
- LLM explanation/synthesis.

## Artifacts

- Day 1 lesson notes prepared
- Open questions prepared
- HealthSure state update prepared
- ADR intentionally deferred

## Recommended Next Topic

Day 2 — Long Context vs Retrieval

Architectural question:

> When does retrieval provide enough value to justify an additional subsystem compared with supplying the necessary context directly to the model?

## Last Updated

2026-08-08
