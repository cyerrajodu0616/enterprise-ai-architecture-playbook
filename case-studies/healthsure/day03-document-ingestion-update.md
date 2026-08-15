# HealthSure Case Study Update — Week 1 Day 3

## Known HealthSure State

The initial use case remains an internal advisory policy knowledge assistant. No production AI architecture is accepted. ADR-0002 remains an Experimental qualified-long-context pilot.

## Previous Position

Day 2 requires deterministic applicability, complete packages, and authorization before policy evidence reaches the model. Retrieval remains deferred.

## Proposed Boundary

ADR-0003 proposes version-aware, structure-aware representations; immutable identities; separate validation, approval, applicability, authorization, and eligibility evidence; atomic package activation; dependency invalidation; and content plus resolver lineage.

This is a logical boundary, not a deployed registry, parser, store, event system, approval workflow, or production architecture.

## Conditional Source Handling

Use authoritative-source originals when immutable historical retrieval and integrity are guaranteed. Otherwise preserve a governed immutable copy. These HealthSure source guarantees are unknown.

## Activation Position

Begin with manual activation and shadow automation. Narrow automatic activation under preapproved rules remains conditional on evidence. Production separation-of-duties requirements remain open.

## New Risks

- meaning lost across tables, footnotes, pages, images, or amendments;
- incomplete package published from individually valid components;
- parser/configuration change leaving inherited eligibility active;
- inapplicable old package used as fallback;
- retroactive metadata correction missing historical impact;
- sensitive replay evidence creating a secondary risk boundary;
- manual review or fallback capacity becoming insufficient.

## Evidence Needed

See `HS-DIM-001` through `HS-DIM-016` in `open-questions/index.md`. Source guarantees, policy semantics, corpus structures, freshness, authorization, retention, owners, review capacity, scale, and validation thresholds remain unknown.

## Next Pressure

Day 4 examines whether and how knowledge should be chunked without destroying the structure and package meaning established on Day 3.
