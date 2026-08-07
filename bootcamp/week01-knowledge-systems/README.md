# Week 1 — Knowledge Systems

**Status:** Not Started

## Architectural Capability

Design how an AI system finds and uses enterprise knowledge.

## Why This Week Exists

Knowledge access makes grounding, evidence, and evaluation concrete before platform complexity is introduced.

## HealthSure Business Trigger

Service agents need an internal assistant that answers policy questions accurately and cites governing policy language.

## Major Topics

- Long context, retrieval, keyword search, semantic search, hybrid search, chunking, metadata, reranking, citations, and retrieval evaluation.

## Decisions to Make

- When is supplied context sufficient, and when is retrieval justified?
- How should documents be chunked, indexed, filtered, cited, and evaluated?
- Which knowledge source is authoritative when policy versions conflict?

## Counterarguments to Explore

- Long context may be simpler than retrieval for a small corpus.
- Keyword search may outperform semantic search for exact policy terms.
- Retrieval infrastructure may cost more to operate than it saves.

## Expected Artifacts

- Retrieval ADRs, an evaluation plan, a HealthSure knowledge-domain model, and the Week 1 architecture diagram.

## Exit Criteria

Explain why retrieval exists, when it is unnecessary, how indexing choices affect quality and cost, and what changes the recommendation.

[Full roadmap](../../ROADMAP.md) · [Bootcamp overview](../README.md)
