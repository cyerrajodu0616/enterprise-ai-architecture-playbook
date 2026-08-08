# ADR-0001 — Enterprise Knowledge Access Pattern for HealthSure v1

- Status: Proposed
- Date: 2026-08-08

## Decision

Do not select one universal AI knowledge-access mechanism yet.

Use question- and source-aware access patterns:
- authoritative structured access for changing operational facts;
- deterministic policy/version scoping;
- direct context/search/retrieval for policy language only when justified;
- no vector database as the canonical system of record for live structured business state.

## Why Proposed, Not Accepted

ADR-0002 records a qualified-long-context pilot as Experimental, but production acceptance, centralized serving, and replica/cache choices still depend on unresolved HealthSure evidence.

## Strongest Counterargument

A unified serving/retrieval layer may simplify integration, protect fragile sources, and improve reproducibility.

## Review Triggers

Revisit when real corpus, traffic, latency, freshness, authorization, source-capacity, and evaluation evidence are available.

## Related Decisions

- ADR-0002 refines the policy-language access path experimentally and does not supersede this proposed question- and source-aware pattern.
