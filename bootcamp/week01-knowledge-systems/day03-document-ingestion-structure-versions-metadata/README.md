# Day 3 — Document Ingestion, Structure, Versions & Metadata

## Status

Reasoning completed. The policy-ingestion and eligibility boundary is Proposed; no production architecture or product selection is accepted.

## 1. Executive Summary

HealthSure's advisory policy knowledge assistant needs more than extracted text. Before evidence reaches the model, the system must establish identity, time, authority, completeness, reproducibility, authorization, and applicability while preserving the structure that carries business meaning.

The conditional recommendation is a version-aware, structure-aware policy representation with immutable source and metadata identities, deterministic package and applicability gates, explicit approval and eligibility states, atomic activation, dependency-based invalidation, and answer-to-evidence plus resolver lineage. The strongest counterargument is that a sufficiently capable authoritative source may already provide these guarantees, making a separate registry and governed copies unnecessary.

ADR-0003 records this boundary as **Proposed**, not Accepted. Exact HealthSure sources, semantics, formats, freshness, retention, owners, review capacity, and evaluation thresholds remain open.

## 2. Business Problem

Service agents need complete governing policy evidence for the correct business context. Supplying the wrong version, an incomplete package, a structurally corrupted table, or an unauthorized artifact can produce a confident but materially wrong explanation.

The business question is not merely whether ingestion extracted text. It is whether HealthSure can prove why the exact evidence was eligible for a particular request and can investigate what should have been used after a correction.

## 3. Current State and Trigger

### Known

- The initial assistant remains internal, advisory, and non-authoritative.
- No production AI architecture is accepted.
- ADR-0002 is an Experimental qualified-long-context pilot only.
- Applicability, complete package validation, and authorization precede model invocation.
- Retrieval remains deferred until evidence earns its obligations.

### Trigger

Day 2 depends on trustworthy packages but does not yet define how policy artifacts are represented, validated, approved, activated, invalidated, replayed, or remediated when metadata changes retroactively.

## 4. Naive Explanation

An ingested document is not trustworthy because a parser produced text. HealthSure must be able to answer:

```text
What source artifact was this?
Which metadata and package relationships applied?
What business meaning survived parsing?
Who approved its authority?
For which context was it applicable and authorized?
Why was this exact representation eligible?
What should have been used after a correction?
```

## 5. Requirements, Constraints, and Assumptions

### Requirements

- Preserve identity, time, authority, completeness, and reproducibility.
- Preserve meaning-bearing structure and package relationships.
- Separate parsing, technical validation, business approval, applicability, authorization, and eligibility.
- Fail closed for unresolved required authoritative content or relationships.
- Activate complete packages atomically and retain applicable history.
- Support dependency-based invalidation, impact analysis, replay, and remediation.
- Minimize sensitive audit duplication while preserving sufficient reconstruction evidence.

### Constraints

- The assistant remains advisory.
- Inapplicable versions and silent package incompleteness remain release blockers.
- Ambiguity fails toward qualification, abstention, or approved manual review.
- No parser, model, or ingestion team may infer legal or business applicability.

### Assumptions

- **Assumption:** A version-aware representation and explicit eligibility boundary can reduce wrong-version and incomplete-package risk.
- **Assumption:** Manual activation and shadow automation are supportable for a controlled pilot.
- **Assumption:** Exact upstream identities and versions can be recorded, even though source capabilities remain unverified.

### Hypotheses

- **Hypothesis:** Structure-aware validation will prevent meaning-bearing table, footnote, and package relationships from being silently lost.
- **Hypothesis:** Resolver lineage will identify retroactively affected answers that content lineage alone cannot find.

### Open Questions

Source guarantees, policy semantics, real formats, ownership, authorization, freshness, retention, review capacity, scale, and evaluation thresholds are unknown. See `HS-DIM-001` through `HS-DIM-016` in the [open-question index](../../../open-questions/index.md).

## 6. The Simplest Viable Solution

First evaluate direct authoritative access. If a source can resolve immutable, approved, applicable, complete historical packages with required integrity, latency, availability, authorization, and capacity, HealthSure may store metadata, checksums, parsed representations, and lineage while retrieving the exact original from that source.

If those guarantees are insufficient, preserve a governed immutable original and maintain a version-aware registry. Begin with manual activation by an accountable policy owner and use shadow automation only to gather evidence. Do not introduce real-time events, broad automatic activation, or a separate product choice without evidence.

## 7. Technical Deep Dive

### Trust and eligibility states

- `PARSED`: conversion preserved required content and structure under versioned parsing rules.
- `TECHNICALLY_VALIDATED`: deterministic integrity, schema, metadata, structure, package, and processing checks passed.
- `BUSINESS_APPROVED`: an accountable policy owner confirmed governing authority.
- `APPLICABLE`: the approved package governs the confirmed business context.
- `ELIGIBLE`: the exact authorized, applicable, approved, complete, validated representation may reach the model.

A perfectly parsed draft is not governing policy.

### Version and dependency model

Preserve separate immutable versions of the source artifact, source metadata, parsed representation, parser/configuration, validation rules/results, authority approval, package manifest, applicability decision, authorization decision, and eligibility decision. Downstream eligibility references the exact upstream versions.

```text
Source artifact + metadata
        ↓
Structure-aware parsed representation
        ↓
Technical validation + package manifest
        ↓
Business approval
        ↓
Applicability + authorization
        ↓
Eligibility decision
        ↓
Qualified evidence supplied to model
        ↓
Answer + evidence lineage + resolver lineage
```

### Structure-aware parsing

Preserve section hierarchy, table cells and headers, footnote targets, pages, images, amendments, and affected provisions. Quarantine handwritten or ambiguous annotations until an accountable owner classifies them. Retention does not automatically make content eligible for answers.

### Package validation and activation

Package completeness comes from an authoritative versioned manifest. Fail closed when a required, authoritative, meaning-bearing component or relationship fails validation. Activate all required components atomically. An old package remains available only for intervals where applicability rules still make it valid.

### Change, invalidation, and freshness

A source change can affect bytes, metadata, effective dates, approval, withdrawal, amendment relationships, package membership, authority, authorization, retention, or source behavior. End-to-end freshness is:

```text
detection + retrieval + parsing + validation + approval + activation
```

A parser change creates a new immutable candidate and invalidates inherited technical validation and eligibility for that representation. It does not automatically revoke business approval of the original source.

### Replay and impact analysis

Content lineage identifies what influenced an answer. Resolver lineage identifies what should have influenced it. Retroactive metadata corrections therefore require historical package resolution, not only searches for answers that used the corrected artifact.

## 8. Architecture Options

### Option A — Direct authoritative resolution

Retrieve exact historical originals from the authoritative source and retain metadata, parsed representations, checksums, and lineage.

- Strength: avoids a duplicate original store.
- Weakness: depends on immutable historical retrieval, integrity, availability, authorization, retention, and export guarantees that are not yet known.

### Option B — Version-aware registry with governed immutable originals

Retain originals plus immutable metadata, representations, manifests, validation, approval, applicability, authorization, and eligibility decisions.

- Strength: stronger replay and source-independence when upstream guarantees are insufficient.
- Weakness: adds storage, security, retention, reconciliation, migration, and operating obligations.

### Option C — Minimal parsed-text pipeline

Store extracted text and current metadata with limited state management.

- Strength: simplest implementation.
- Weakness: cannot satisfy the proposed correctness boundary when structure, package relationships, retroactive applicability, or replay matters.

## 9. Counterarguments and Devil's Advocate

The proposed boundary may overbuild a pilot before HealthSure knows its corpus or source guarantees. An authoritative repository with immutable version access and an accountable owner could satisfy most requirements with fewer components. Manual activation can also become a bottleneck, and extensive lineage can create a sensitive secondary evidence system.

The response is conditional: preserve the boundary and required evidence, but realize it with the smallest mechanism the authoritative sources can support. Do not equate the logical boundary with a mandate for a new platform.

## 10. Trade-off Analysis

- Reproducibility versus sensitive-data duplication.
- Fail-closed correctness versus availability and manual-review load.
- Structure preservation versus parser and validation complexity.
- Automatic activation speed versus accountable human control.
- Immutable history versus storage, retention, and deletion obligations.
- Narrow impact analysis versus the risk of missing affected answers.

## 11. Cost Model

Exact HealthSure costs are unknown. Relevant variables include:

- source integration and export;
- original and derived storage;
- parsing/OCR and validation;
- manifest and resolver engineering;
- human classification, approval, and review;
- reprocessing after parser or metadata changes;
- lineage, audit, access control, monitoring, and incidents;
- remediation of retroactively affected answers.

The dominant cost may be review and remediation rather than compute. Measure cost per eligible, correct, reproducible answer—not ingestion cost alone.

## 12. Operational Concerns

- Assign ownership separately for source authority, ingestion, parsing, validation, package administration, policy approval, applicability, authorization, and serving.
- Monitor source changes, validation failures, incomplete packages, activation backlog, suspension scope, replay backlog, and manual fallback.
- Roll out parser changes as governed migrations with immutable candidates, comparison, atomic activation, and rollback.
- Suspend identifiable affected representations when a material defect is known; fail closed more broadly when the affected scope cannot be established.
- Retain reconciliation even if future events are introduced because events can be missed, duplicated, delayed, or reordered.

## 13. Security, Privacy, and Governance

- Enforce least privilege for originals, parsed content, restricted audit evidence, and token vaults.
- Separate operational telemetry from restricted evidence and re-identification mappings.
- Retain only member/claim fields that materially influenced applicability, authorization, evidence, or the answer.
- Record protected historical source identity, version/timestamp, schema, checksum, and authorization evidence where required.
- Keep retention periods, legal-hold behavior, deletion, and separation-of-duties requirements open until accountable HealthSure owners define them.

## 14. Evaluation and Evidence

Validate on actual HealthSure documents when available:

- structure and relationship preservation;
- table/header and footnote association;
- package completeness and precedence;
- version and effective-date resolution;
- immutable historical retrieval and checksum verification;
- dependency invalidation and atomic activation;
- retroactive impact analysis using resolver lineage;
- authorization and sensitive-evidence handling;
- manual and shadow-automation outcomes;
- rollback, suspension, abstention, and fallback behavior.

Parser confidence is diagnostic evidence, not business correctness. Acceptance criteria remain open.

## 15. Final Recommendation

> Under the current assumptions and constraints, HealthSure should use a version-aware, structure-aware policy representation with immutable source and metadata identities, deterministic package-completeness and applicability gates, explicit approval and eligibility states, atomic activation, dependency-based invalidation, and exact answer-to-evidence and resolver lineage. It should rely on authoritative-source originals only when immutable historical retrieval and integrity are guaranteed; otherwise it should retain a governed copy. It should begin with manual activation and shadow automation, using automatic activation only for narrowly approved routine cases after evidence justifies it. We accept additional validation, lineage, review, and abstention obligations to reduce wrong-version, incomplete-package, structural-corruption, and irreproducibility risks. We would revisit the design when real source guarantees, policy semantics, document structures, freshness requirements, authorization boundaries, scale, review capacity, and evaluation evidence are available.

This recommendation and ADR-0003 are Proposed. No production architecture is accepted.

## 16. What Would Change the Recommendation?

- authoritative sources proving or failing immutable historical retrieval and integrity;
- verified policy version, correction, withdrawal, amendment, and precedence semantics;
- actual document structures and parser-validation evidence;
- business-approved freshness, retention, authorization, and review requirements;
- source latency, availability, capacity, and export evidence;
- measured manual-review load and shadow-automation quality;
- scale or incident evidence that changes the simplest viable implementation.

## 17. HealthSure Case-Study Update

Day 3 proposes a logical ingestion and eligibility boundary; it does not deploy a registry, parser, store, event system, or automated approval process. HealthSure now distinguishes source authority from derived-representation validation and records the need for content plus resolver lineage. See the [Day 3 case-study update](../../../case-studies/healthsure/day03-document-ingestion-update.md).

## 18. Architect's Challenge

See [Architect's Challenge](architects-challenge.md): address a retroactive applicability correction, incomplete new package, historical answers, source constraints, and an effective-date boundary without assuming a single correct implementation.

## 19. Architect's Reflection

The learner remains least comfortable approving automatic activation without evidence. Parser confidence, partial publication, metadata corrections, resolver lineage, and proportionate pilot separation of duties became central concerns. See the [Day 3 reflection](../../../architect-journal/week01-day03-reflection.md).

## Architecture Reasoning Journal

### Decision Point 1 — Correctness dimensions

**Mentor question:** What minimum information must HealthSure preserve to prove why a policy document or package was supplied for a service date?

**Learner's initial reasoning:** All five dimensions are required: identity, time, authority, completeness, and reproducibility.

**Challenge:** These are requirements, not proof that every possible ingestion component is needed. A version-aware registry or sufficiently capable authoritative source could satisfy them.

**Refined reasoning:** Preserve stable policy/source identities, business-effective and system-observed time, authority and approval, package relationships, immutable version/checksum, parser version, validation, and lineage. Exact HealthSure fields remain open.

**Principle learned:** A document is not trustworthy AI evidence merely because its text was extracted.

**Change trigger:** Confirmed HealthSure source and policy semantics.

### Decision Point 2 — Preserve originals or references

**Mentor question:** Store every immutable source artifact, or retain metadata and parsed representations while retrieving originals from the source?

**Learner's initial reasoning:** Store metadata and parsed representation with complete lineage, retrieving the original from its source.

**Challenge:** Lineage cannot reproduce an object that was overwritten, deleted, mutated, or cannot be retrieved historically.

**Refined reasoning:** Use references only when the source guarantees immutable, version-addressable, checksum-verifiable historical retrieval for the required period. Otherwise preserve a governed immutable copy.

**Principle learned:** A pointer is not evidence unless the referenced object is immutable and durably retrievable.

**Change trigger:** Source retention, integrity, legal-hold, availability, authorization, and export evidence.

### Decision Point 3 — Structure-aware parsing and handwritten annotations

**Mentor question:** Produce plain text, a structure-aware representation, or both; and how should handwriting be handled?

**Learner's initial reasoning:** Use a structure-aware representation and first determine the content and authority of handwritten annotations.

**Challenge:** Handwriting is neither automatically irrelevant nor governing; extraction can preserve words while destroying table, footnote, section, or page relationships.

**Refined reasoning:** Preserve semantic and positional relationships. Quarantine ambiguous annotations until an accountable owner classifies them; retain for audit when required without making them answer-eligible.

**Principle learned:** Parsing preserves business meaning, not merely characters. Retention and eligibility are separate.

**Change trigger:** Actual corpus structures and accountable content classification.

### Decision Point 4 — Parser uncertainty and package failure

**Hypothetical scenario:** Paragraphs parse, but a coverage-table boundary and footnote target are uncertain and an approval page has low OCR confidence.

**Learner's initial reasoning:** Reject the entire document version because confidence is not business correctness and partial publication can destroy correlations.

**Challenge:** Requiring perfect extraction of decorative or preclassified non-governing content may unnecessarily reduce availability.

**Refined reasoning:** Fail closed for required, authoritative, meaning-bearing content or relationships. Define completeness through an authoritative versioned manifest. Exclude non-required objects only under preapproved rules.

**Principle learned:** Parser confidence is diagnostic evidence; component validity does not prove package completeness; accountable owners define materiality.

**Change trigger:** Approved manifest and materiality rules validated against actual documents.

### Decision Point 5 — Validation, approval, applicability, and eligibility

**Mentor question:** What do successful parsing and technical validation mean, and when may an artifact influence an answer?

**Learner's initial reasoning:** Store technically validated versions but require policy-domain approval before answer eligibility.

**Challenge:** A technically perfect draft may still be non-governing, and successful parsing needs explicit versioned rules.

**Refined reasoning:** Keep `PARSED`, `TECHNICALLY_VALIDATED`, `BUSINESS_APPROVED`, `APPLICABLE`, and `ELIGIBLE` as separate evidence-bearing states. Initial activation requires policy-domain approval.

**Principle learned:** Technical correctness does not establish governing authority or business applicability.

**Change trigger:** Mature evidence for narrowly preapproved activation rules.

### Decision Point 6 — Dependency-based invalidation

**Mentor question:** What must be revisited when an upstream representation changes?

**Learner's initial reasoning:** Downstream steps depending on the change must be revisited; parser changes affect validation and approval/eligibility.

**Challenge:** A parser change need not revoke business approval of the original source.

**Refined reasoning:** Separate source approval from derived validation. New representations lose inherited technical validation and eligibility while source authority can remain intact. Reference exact immutable upstream versions.

**Principle learned:** A parser upgrade is a governed data migration, not merely a library update.

**Change trigger:** Verified dependency graph and change-impact rules.

### Decision Point 7 — Parser upgrade rollout

**Hypothetical scenario:** A parser upgrade affects many validated representations.

**Learner's initial reasoning:** Keep existing validated representations active while new candidates are built and validated unless a material old-parser defect is known.

**Challenge:** Existing output is not proven valid forever; known defects can require suspension.

**Refined reasoning:** Create immutable candidates, compare meaning-bearing structure, activate atomically after validation and approval, preserve old history, and suspend identifiable affected scope for material defects. Fail closed more broadly if scope is unknowable.

**Principle learned:** Normally only one representation is active per source version and serving scope, with explicit migration and rollback.

**Change trigger:** Defect materiality, affected-scope evidence, and candidate validation.

### Decision Point 8 — Detecting source changes and freshness

**Mentor question:** Poll, read directly, or introduce change events?

**Learner's initial reasoning:** Identify change signals and acceptable freshness; use batch polling for bounded staleness, or direct access when source latency and permission boundaries permit.

**Challenge:** Changes include metadata, approval, package, authority, authorization, retention, and source behavior—not bytes alone.

**Refined reasoning:** Evaluate direct authoritative access first; otherwise use scheduled polling and a version-aware registry with cadence derived from approved freshness. Do not add real-time events without evidence, and retain reconciliation if events arrive later.

**Principle learned:** Freshness is the full detection-to-activation path.

**Change trigger:** Business-approved freshness plus source latency, capacity, availability, and change evidence.

### Decision Point 9 — Automatic versus manual activation

**Mentor question:** Which updates may activate without direct human action?

**Learner's initial reasoning:** Use a hybrid model with owners deciding automatic and manual cases within their boundaries.

**Challenge:** HealthSure lacks a verified change taxonomy and complete policy semantics, so automatic activation is unsafe.

**Refined reasoning:** Start manual, then shadow recommendations, narrow automatic activation under preapproved rules, and expand only after measured evidence. Each owner certifies only their boundary.

**Principle learned:** Automatic activation is not automatic business approval.

**Change trigger:** Confirmed semantics, shadow evidence, rollback, named owners, fallback capacity, and approved criteria.

### Decision Point 10 — Atomic package activation

**Hypothetical scenario:** A base policy, amendments, and required attachments form a package, but one new attachment remains under review.

**Learner's initial reasoning:** Keep the existing complete package active.

**Challenge:** A complete old package becomes wrong when its applicability interval ends.

**Refined reasoning:** Activate required components atomically. Keep the old package only where still applicable. If the new effective date arrives before completeness, abstain for the affected scope and use an approved manual fallback.

**Principle learned:** A previous version is a valid fallback only while applicability rules make it valid.

**Change trigger:** Verified applicability intervals, complete manifest, and fallback capacity.

### Decision Point 11 — Retroactive amendments and historical answers

**Mentor question:** What happens when a retroactive amendment changes historical applicability?

**Learner's initial reasoning:** Use it for future queries and rerun queries that used the prior version.

**Challenge:** Future execution can ask about historical dates, and blindly rerunning every old answer is unnecessarily broad.

**Refined reasoning:** Identify the smallest defensible scope using package, dates, product/jurisdiction, relevant dimensions, affected provisions, dependencies, question category, and exact evidence. Reconstruct and compare before routing material differences for remediation; preserve both histories.

**Principle learned:** Lineage supports impact analysis and remediation, not only audit.

**Change trigger:** Confirmed retroactive scope and materiality rules.

### Decision Point 12 — Replay and audit evidence

**Mentor question:** What must be retained to investigate or reconstruct an answer?

**Learner's initial reasoning:** Retain everything required to reconstruct the answer.

**Challenge:** Indefinite duplication of prompts and sensitive data creates another high-risk system.

**Refined reasoning:** Retain a governed execution envelope containing applicability and authorization inputs, exact package/representation identities, resolver/manifest/parser/validation/configuration versions, prompt/model configuration, justified evidence, answer/citations, fallback, and remediation history.

**Principle learned:** Reproduce decision inputs and investigate outcomes; do not promise identical generated wording.

**Change trigger:** Approved retention, deletion, legal-hold, and audit requirements.

### Decision Point 13 — Member-specific data retention

**Mentor question:** How should member/claim context be retained without creating an unnecessary sensitive copy?

**Learner's initial reasoning:** Retain tokenized or redacted prompts with protected references to authoritative claim records.

**Challenge:** A mutable claim ID alone cannot reconstruct historical state.

**Refined reasoning:** Reference protected exact historical state with version/timestamp, schema, checksum, and authorization evidence. Store a minimal governed snapshot only when the source cannot reproduce required fields; separate telemetry, restricted evidence, and token mapping.

**Principle learned:** Retain only sensitive fields that materially influenced the decision boundary.

**Change trigger:** Historical source reproducibility and approved evidence-retention requirements.

### Decision Point 14 — Effective-date correction without content change

**Hypothetical scenario:** An amendment's effective date is corrected to cover additional historical months while its bytes remain unchanged.

**Learner's initial reasoning:** Treat it as metadata-only and use lineage for impact analysis.

**Challenge:** Answers that should have used the amendment may not appear in content lineage precisely because the old metadata excluded it.

**Refined reasoning:** Create immutable metadata history, reuse valid content validation, revalidate authority when needed, recalculate applicability/eligibility, rerun historical resolution, compare packages, and investigate the corrected business scope.

**Principle learned:** Content lineage shows what influenced an answer; resolver lineage shows what should have influenced it.

**Change trigger:** Confirmed correction semantics and historical resolver inputs.

### Decision Point 15 — Control simplification

**Mentor question:** Which control is hardest to approve, and what can be simplified without weakening pilot correctness?

**Learner's initial reasoning:** Automatic activation is least comfortable without evidence.

**Challenge:** Full separation of duties for every routine pilot update may add process without demonstrated value.

**Refined reasoning:** During a controlled pilot, one accountable policy owner may approve and activate routine fully validated packages; high-risk exceptions require a second independent reviewer. Production separation-of-duties remains open.

**Principle learned:** Governance should be proportionate to evidence and risk, not ceremonial.

**Change trigger:** HealthSure governance requirements, change taxonomy, review outcomes, and incident evidence.

## Next Lesson

Day 4 — Chunking as an Architectural Decision

> How should retrieval units reflect the structure of knowledge and the questions users actually ask?

---

**Navigation:** [Previous: Day 2](../day02-long-context-vs-retrieval/README.md) · [Week 1 overview](../README.md) · Next: Day 4 — Chunking as an Architectural Decision
