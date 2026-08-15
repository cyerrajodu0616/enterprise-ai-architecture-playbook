# Planner Prompt — Generate Week 1 Day 3 Document Ingestion Artifacts

## Feature context

Week 1 Day 3 reasoning is complete. The repository currently ends with Day 2 and an Experimental qualified-long-context pilot. Day 3 must preserve the learner's reasoning about trustworthy policy ingestion, structure, versions, metadata, eligibility, activation, dependency invalidation, retroactive corrections, and replay without inventing HealthSure facts or accepting a production architecture.

The architectural question is:

> How do we ensure the model receives the correct version of complete, authoritative information?

## Governing sources

- `ARCHITECTURE_MANIFESTO.md:1`
- `PLAYBOOK_GUIDELINES.md:20`
- `PLAYBOOK_GUIDELINES.md:276`
- `PLAYBOOK_GUIDELINES.md:290`
- `PLAYBOOK_GUIDELINES.md:324`
- `PLAYBOOK_GUIDELINES.md:498`
- `ROADMAP.md:34`
- `CURRENT_SESSION.md:3`
- `bootcamp/week01-knowledge-systems/README.md:77`
- `bootcamp/week01-knowledge-systems/day02-long-context-vs-retrieval/README.md:1`
- `case-studies/healthsure/current-state.md:32`
- `adr/0002-qualified-long-context-pilot.md:11`

## Files to create

1. `bootcamp/week01-knowledge-systems/day03-document-ingestion-structure-versions-metadata/README.md`
2. `bootcamp/week01-knowledge-systems/day03-document-ingestion-structure-versions-metadata/index.html`
3. `bootcamp/week01-knowledge-systems/day03-document-ingestion-structure-versions-metadata/architects-challenge.md`
4. `adr/0003-version-aware-policy-ingestion-eligibility-boundary.md`
5. `cheatsheets/week01-day03-document-ingestion.md`
6. `case-studies/healthsure/day03-document-ingestion-update.md`
7. `architect-journal/week01-day03-reflection.md`

The Markdown and HTML lessons must include all required playbook sections plus the full 15-point Architecture Reasoning Journal supplied by the user. Preserve initial learner answers separately from mentor challenges and refined positions. Label numerical scenarios as hypothetical and uncertainty as Known, Assumption, Open Question, or Hypothesis.

## Files to update

### `CURRENT_SESSION.md:3-42`

Replace the Day 2 handoff with Day 3 completion, the Day 3 reasoning shifts, Proposed ADR-0003, conditional decisions, unresolved evidence, artifact list, and Day 4 as the next topic. State explicitly that no production architecture was accepted.

### `bootcamp/week01-knowledge-systems/README.md:3`

Before:

```markdown
**Status:** In Progress — Days 1 and 2 completed; next is Day 3
```

After:

```markdown
**Status:** In Progress — Days 1–3 completed; next is Day 4
```

Link the existing Day 3 heading at line 77 to the new lesson directory. Preserve its purpose, exploration topics, question, and all other week content. Add next/previous links between Day 2 and Day 3.

### `case-studies/healthsure/current-state.md:42`

Insert a cumulative Day 3 update before Immediate Architectural Work. Preserve Days 0–2. Record the version-aware, structure-aware eligibility boundary as Proposed, not deployed. State all source guarantees, semantics, formats, freshness, ownership, authorization, retention, review capacity, and evaluation thresholds as open evidence.

### `case-studies/healthsure/open-decisions.md:5`

Preserve existing decisions and add a conditional Day 3 entry for version-aware policy ingestion, immutable-source retention, manual/shadow activation, and unresolved production controls.

### `adr/index.md:6`

Append ADR-0003 with status Proposed, Day 3 lesson link, and review date `2026-08-15`.

### `open-questions/index.md:22`

Preserve all existing questions and append Day 3 questions using IDs `HS-DIM-001` through `HS-DIM-016`. Every status remains Open and no unknown answer is invented.

## ADR judgment

Create ADR-0003, **Version-Aware Policy Ingestion and Eligibility Boundary**, with status **Proposed**. The reasoning establishes a coherent architectural boundary worth preserving, but source guarantees and production evidence remain unknown. It must not select a vendor, product, database, parser, event system, storage platform, polling interval, retention period, approval threshold, or deployment architecture.

## Constraints and gotchas

- Preserve Day 1 and Day 2 positions and cumulative HealthSure state.
- No production architecture is accepted.
- ADR-0002 remains Experimental.
- Retrieval remains deferred.
- Do not invent source capabilities, formats, owners, policy semantics, freshness, scale, traffic, cost, retention, regulations, parser accuracy, review capacity, or automation thresholds.
- Parser confidence is diagnostic evidence, not business approval.
- A pointer is evidence only when exact immutable historical retrieval and integrity are guaranteed.
- Partial validity does not establish package completeness.
- Applicability, business approval, technical validation, authorization, and eligibility are separate states.
- Parser upgrades create new immutable derived representations and invalidate downstream technical validation/eligibility, not necessarily source authority approval.
- Automatic activation means activation under preapproved rules and begins only after manual and shadow stages.
- Old packages remain fallback only while applicable.
- Content lineage and resolver lineage serve different impact-analysis needs.
- Audit retention must minimize sensitive duplication and remains governed by unknown HealthSure retention rules.
- Do not begin Day 4.

## Validation

- `git diff --check`
- verify all Markdown and HTML relative links;
- verify HTML is self-contained, responsive, and structurally complete;
- verify all 15 journal decisions appear in Markdown and HTML;
- verify all 16 Day 3 open questions remain Open;
- verify ADR-0003 and index both say Proposed;
- search for unsupported numerical HealthSure claims and production acceptance;
- confirm only intended Day 3 files, navigation/index updates, and this prompt file changed.
