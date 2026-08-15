# ADR-0003 — Version-Aware Policy Ingestion and Eligibility Boundary

- Status: Proposed
- Date: 2026-08-15
- Owners: HealthSure policy-domain, source-system, ingestion, security, governance, and operations owners (assignments remain to be confirmed)

## Business Context

The Experimental qualified-long-context pilot depends on complete, authoritative, applicable policy evidence. HealthSure has not verified its source guarantees, policy semantics, document structures, freshness, authorization, retention, review capacity, or production thresholds.

## Decision Scope

Define the logical boundary that makes a policy representation eligible for an advisory model. This ADR does not choose a product, database, parser, event system, storage platform, polling interval, retention period, approval threshold, or production deployment.

## Assumptions

- Explicit immutable identities and evidence-bearing states can reduce wrong-version and incomplete-package risk.
- Manual activation and shadow automation can support an initial controlled pilot.
- Upstream source and policy semantics can be confirmed before implementation acceptance.

All assumptions remain unverified HealthSure evidence.

## Constraints

- The assistant remains advisory.
- Applicability, package completeness, authorization, and eligibility precede model invocation.
- Materially wrong, historically inapplicable, or silently incomplete evidence remains a release blocker.
- Ambiguity fails toward qualification, abstention, or approved manual review.

## Options Considered

1. Direct immutable historical resolution from authoritative sources with derived representations and lineage.
2. A version-aware registry retaining governed immutable originals when source guarantees are insufficient.
3. A minimal parsed-text pipeline without explicit eligibility and dependency evidence.
4. Immediate event-driven ingestion and automatic activation.

## Decision Drivers

- governing authority and temporal applicability;
- package completeness and structure preservation;
- reproducibility, impact analysis, and remediation;
- authorization and sensitive evidence;
- operational simplicity and review capacity;
- reversibility and source independence.

## Decision

Propose a version-aware, structure-aware policy representation with immutable source and metadata identities; deterministic package-completeness, applicability, and authorization gates; separate parsed, technically validated, business-approved, applicable, and eligible states; atomic package activation; dependency-based invalidation; and answer-to-evidence plus resolver lineage.

Use authoritative-source originals only when immutable, version-addressable, checksum-verifiable historical retrieval and integrity are guaranteed for the required period. Otherwise preserve a governed immutable copy. Begin with manual activation and shadow automation; permit narrow automatic activation only under preapproved rules after evidence justifies it.

## Why Proposed, Not Accepted

The reasoning establishes a durable boundary, but HealthSure has not confirmed source capabilities, package semantics, formats, ownership, freshness, authorization, retention, review capacity, scale, or evaluation evidence. No production component is accepted.

## Strongest Counterargument

A capable authoritative repository and accountable policy owner may already satisfy the boundary with fewer components. A separate registry, governed copies, extensive lineage, and manual activation may add cost, sensitive duplication, and bottlenecks before the real corpus demonstrates need.

## Trade-offs Accepted

Accept additional validation, lineage, review, abstention, and migration obligations to reduce wrong-version, incomplete-package, structural-corruption, unauthorized-evidence, and irreproducibility risks.

## Consequences

- Parser upgrades become governed migrations of derived representations.
- Package completeness comes from an authoritative versioned manifest.
- Required meaning-bearing validation failures fail closed.
- Only applicable complete packages activate atomically.
- Retroactive metadata changes trigger resolver-based impact analysis.
- Old packages remain available only for contexts where they still apply.

## Cost Implications

Costs include integration, storage, parsing/OCR, validation, manifests, approvals, reprocessing, lineage, audit, security, review, remediation, and incidents. Exact cost and dominant drivers remain unknown.

## Operational Impact

Define ownership, monitoring, activation queues, suspension, rollback, reconciliation, manual fallback, replay, and remediation. Preserve exact upstream versions and eligibility decisions.

## Security and Governance Impact

Separate telemetry, restricted audit evidence, and re-identification mappings. Minimize member/claim data. Retention, legal hold, deletion, and separation-of-duties requirements remain open.

## Validation Plan

- verify structure and relationship preservation on actual documents;
- test package completeness, version, precedence, and applicability;
- prove historical retrieval and checksum behavior;
- exercise parser migrations, atomic activation, suspension, and rollback;
- test retroactive corrections with content and resolver lineage;
- run authorization, sensitive-retention, manual-review, and shadow-automation evaluations.

## Review Triggers

- confirmed or failed source historical-retrieval guarantees;
- verified policy and package semantics;
- actual document formats and parser evidence;
- approved freshness, authorization, retention, and review requirements;
- material parser or resolver defect;
- measured scale, cost, availability, or manual-review constraints;
- shadow automation evidence.

## Superseded Decisions

None.

## Related Decisions

- Refines the ingestion and eligibility boundary required by Experimental ADR-0002.
- Does not supersede ADR-0001 or ADR-0002.
