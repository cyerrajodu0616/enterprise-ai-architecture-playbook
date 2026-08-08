# HealthSure Case Study Update — Week 1 Day 2

## Known HealthSure State

The initial use case remains an internal advisory policy knowledge assistant for service agents. No production AI component has been accepted. Real corpus, workload, latency, cost, policy-version, authorization, and evaluation evidence remains open.

## Previous Position

Day 1 deferred long context versus retrieval and rejected a universal vector database as the canonical store for live structured business state.

## Experimental Decision

ADR-0002 records a bounded qualified-long-context pilot as Experimental. Deterministic applicability and complete package validation must precede model invocation. Retrieval remains deferred until measured limits of the simpler design justify its additional obligations.

No real HealthSure pilot is claimed. All numerical Day 2 corpus, traffic, latency, token, accuracy, and pilot scenarios were hypothetical learning exercises.

## Components and Boundaries

- deterministic product, jurisdiction, date, version, amendment, precedence, and authorization resolution;
- complete policy-package manifest validation;
- qualified context assembly;
- advisory model explanation with citations or abstention;
- evaluation, audit, manual review, and rollback boundaries.

No retrieval, vector, embedding, or reranking component is accepted.

## New Risks

- inapplicable historical policy entering context;
- incomplete base-policy, amendment, or benefits-schedule package;
- governing evidence present but overlooked;
- unsafe date inference;
- wrong claim-context display;
- accepted answers mistaken for independently validated answers;
- token and latency cost growing before retrieval is justified;
- cache reuse across authoritative state changes.

## Metrics Needed

- material-answer and wrong-version rates;
- package completeness, limitation recall, citation correctness, and abstention;
- research-time and handle-time effect;
- correction effort and manual-review burden;
- p50, p95, and p99 end-to-end latency by question class;
- tokens and cost per successful answer;
- background-result use, duplication, and abandonment causes.

## Open Questions

See HS-LCR-001 through HS-LCR-007 in `open-questions/index.md`.

## Next Pressure

Day 3 must establish how documents are parsed, structured, versioned, related, classified, validated for completeness, and traced to authoritative sources.
