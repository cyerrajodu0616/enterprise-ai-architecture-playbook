# ADR-0002 — Qualified Long Context Pilot for HealthSure Policy Knowledge

- Status: Experimental
- Date: 2026-08-08
- Owners: HealthSure policy knowledge product, architecture, policy-domain, security, and operations owners (ownership assignments remain to be confirmed)

## Business Context

HealthSure's first use case is an advisory internal assistant that helps service agents find governing policy language and understand its source. The repository does not yet establish real corpus size, traffic, latency requirements, policy-version semantics, authorization boundaries, total cost, or evaluation thresholds.

## Decision Scope

This ADR governs only the experimental policy-language context path for a bounded pilot. It does not accept a production architecture, retrieval platform, operational-data access pattern, or automated coverage decision.

## Assumptions

- An applicable authorized policy package can be resolved deterministically.
- The qualified package can fit within usable model context for the bounded pilot.
- A controlled pilot can be constrained, monitored, and rolled back.

These assumptions are unverified HealthSure evidence. Numerical scenarios from Day 2 were hypothetical learning exercises.

## Constraints

- The assistant remains advisory.
- Applicability, version, precedence, package completeness, and authorization must be established before model invocation.
- Materially wrong answers, inapplicable historical versions, and silently incomplete packages are release blockers.

## Options Considered

1. Qualified long context after deterministic package validation.
2. Filtered retrieval within a deterministically qualified policy scope.
3. Deterministic complete-section assembly without semantic retrieval.
4. Delay all pilot activity until every production variable is known.

## Decision Drivers

- correctness and evidence completeness;
- simplicity and reversibility;
- business value and agent workflow;
- latency and context capacity;
- total cost of ownership;
- authorization, lineage, and auditability;
- team operating capability.

## Decision

Run a bounded, advisory qualified-long-context pilot after deterministic applicability and complete policy-package validation. Keep retrieval deferred until measured quality, latency, context-capacity, authorization, corpus-growth, or total-cost evidence demonstrates a material net improvement after retrieval's full obligations are included.

## Why Experimental, Not Accepted

No real HealthSure pilot has occurred, and no production component is accepted. Corpus, traffic, latency, cost, version semantics, authorization, business outcome, and safety thresholds remain open evidence.

## Strongest Counterargument

Retrieval may materially reduce context size and latency and improve model attention at sufficient scale. Qualified long context may merely move complexity into token cost and interpretation failures. However, retrieval adds evidence omission, version filtering, ingestion, indexing, reprocessing, evaluation, and on-call responsibilities.

## Trade-offs Accepted

Accept potentially higher model-input cost, latency, and attention limitations to gain a simpler initial operating model, complete qualified context, lower retrieval-omission risk, and faster evidence collection.

## Consequences

- A deterministic resolver and policy-package manifest become correctness boundaries.
- The pilot must abstain when applicability or completeness cannot be established.
- Retrieval infrastructure is deliberately absent.
- Long-context limitations must be measured rather than assumed.

## Cost Implications

Model-input tokens may dominate variable cost. Resolver, assembly, evaluation, review, observability, security, and incidents remain operating costs. Compare cost per correct, useful answer with retrieval's complete engineering and operating cost.

## Operational Impact

Define owners, release gates, incident investigation, claim-scoped context, audit evidence, manual review, disablement, and rollback. Preserve resolver decisions, policy-package identity, model/configuration, context, answer, and citations.

## Security and Governance Impact

Enforce authorization and applicability before context assembly. Prevent unnecessary claim data in notifications and logs. Separate audit evidence from caches. Retain version, lineage, and decision evidence under governed policies that remain to be defined.

## Validation Plan

- golden, held-out, and adversarial policy/version evaluations;
- independent review of accepted and corrected answers;
- material-answer, limitation-recall, citation, wrong-version, and abstention metrics;
- research-time, handle-time, usefulness, correction-effort, and manual-review outcomes;
- end-to-end latency, token use, and cost per successful answer;
- controlled cohort, rollback, and incident review.

## Review Triggers

- measured business outcomes;
- any material answer error or historical-version contamination;
- package-completeness failure;
- context-capacity or authorization limit;
- unacceptable latency or cost per successful answer;
- corpus, product, or jurisdiction growth;
- retrieval comparison evidence showing material net improvement.

## Superseded Decisions

None.

## Related Decisions

This ADR refines but does not supersede ADR-0001.
