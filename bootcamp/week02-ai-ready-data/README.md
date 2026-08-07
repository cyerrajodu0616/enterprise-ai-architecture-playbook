# Week 2 — AI-Ready Data Foundations

**Status:** Not Started

## Architectural Capability

Design trustworthy data foundations for AI decisions and reproducible answers.

## Why This Week Exists

AI quality cannot exceed the authority, history, semantics, and accessibility of the underlying enterprise data.

## HealthSure Business Trigger

The assistant must combine policy documents with structured policy, claims, customer, provider, and product data that changes over time.

## Major Topics

- Data domains, ownership, batch and streaming, canonical models, effective dating, lineage, quality, structured access, and document search.

## Decisions to Make

- Which sources are authoritative, and who owns their meaning and quality?
- When are batch updates sufficient, and when is streaming justified?
- When should the system use SQL or APIs instead of document retrieval?

## Counterarguments to Explore

- A universal canonical model may slow domain delivery.
- Streaming may add cost without improving the business outcome.
- Copying data for AI may weaken governance and reproducibility.

## Expected Artifacts

- Data-domain map, source-of-truth decisions, lineage design, access-pattern ADRs, and a Week 2 architecture update.

## Exit Criteria

Defend data ownership, freshness, history, and access choices while distinguishing justified data capabilities from unnecessary platform work.

[Full roadmap](../../ROADMAP.md) · [Bootcamp overview](../README.md)
