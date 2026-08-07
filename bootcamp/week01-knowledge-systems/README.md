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

## Planned Learning Sequence

This sequence is intentionally provisional. The objective is not to complete seven lessons in seven calendar days. Each lesson continues until the architectural reasoning is sufficiently understood. Lessons may expand, combine, or shift when an important knowledge gap is discovered. Depth takes priority over schedule.

### Day 1 — How Should AI Access Enterprise Knowledge?

**Purpose:** Start from the business problem rather than assuming RAG or retrieval.

**Explore:**

- direct or supplied context;
- structured access through SQL or APIs;
- traditional search;
- semantic retrieval;
- combinations of these approaches.

**Architectural question:**

> What is the simplest knowledge-access architecture that satisfies the HealthSure business requirement?

The lesson should establish requirements and assumptions before selecting an architecture. Do not assume retrieval is required.

### Day 2 — Long Context vs Retrieval

**Purpose:** Determine when retrieval earns its additional complexity compared with supplying the necessary context directly to the model.

**Explore:**

- corpus size;
- context-window capacity;
- token economics;
- latency;
- relevance;
- document change frequency;
- authorization;
- citations;
- operational complexity.

**Architectural question:**

> When does retrieval provide enough value to justify an additional subsystem?

The counterargument must remain explicit: modern large-context models may eliminate the need for retrieval for some workloads.

### Day 3 — Document Ingestion, Structure, Versions & Metadata

**Purpose:** Understand what must happen before enterprise documents become trustworthy AI knowledge sources.

**Explore:**

- parsing;
- document structure;
- tables and images;
- metadata;
- policy versions;
- effective dates;
- authoritative sources;
- lineage;
- freshness.

**Architectural question:**

> How do we ensure the model receives the correct version of authoritative information?

### Day 4 — Chunking as an Architectural Decision

**Purpose:** Treat chunking as a consequence of document structure, retrieval behavior, and user questions rather than a fixed token-size configuration.

**Explore:**

- semantic boundaries;
- question patterns;
- answer completeness;
- parent and child chunks;
- overlap;
- document structure;
- embedding behavior;
- retrieval precision and recall.

**Architectural question:**

> How should retrieval units reflect the structure of knowledge and the questions users actually ask?

This lesson must explicitly challenge whether chunking is required at all for every knowledge source.

### Day 5 — Keyword vs Semantic vs Hybrid Search

**Purpose:** Choose retrieval mechanisms based on query behavior rather than technology popularity.

**Explore:**

- lexical search;
- semantic similarity;
- identifiers and exact terms;
- domain vocabulary;
- metadata filtering;
- hybrid retrieval.

**Architectural question:**

> Which retrieval mechanism best matches the types of questions HealthSure users ask?

The counterargument must remain explicit: traditional search may be sufficient for some enterprise knowledge workloads.

### Day 6 — Reranking, Context Assembly, Citations & Evaluation

**Purpose:** Move beyond retrieving chunks and determine whether retrieved evidence actually supports trustworthy answers.

**Explore:**

- candidate retrieval;
- reranking;
- context construction;
- deduplication;
- citations;
- retrieval evaluation;
- answer evaluation;
- failure analysis.

**Architectural question:**

> How do we know the knowledge system is retrieving enough of the right evidence to support a trustworthy answer?

Evaluation must distinguish retrieval failure from generation failure.

### Day 7 — Architecture Review: HealthSure Knowledge System v1

**Purpose:** Integrate the week's reasoning into a defensible architecture recommendation.

**Review:**

- business requirements;
- assumptions;
- authoritative knowledge sources;
- knowledge-access patterns;
- ingestion;
- retrieval, if justified;
- evaluation;
- security boundaries;
- cost;
- operations;
- unresolved risks.

Challenge the architecture from these perspectives:

- Product;
- Principal Engineering;
- Security;
- Operations/SRE;
- Governance/Legal;
- Finance.

**Architectural question:**

> Under the evidence and assumptions established during Week 1, what should HealthSure Knowledge System v1 contain — and what should it deliberately not contain?

Do not force an ADR simply because the week ended. If evidence remains insufficient for an important decision, record:

- assumptions;
- open questions;
- required experiments;
- decision status as deferred.

### Week 1 Learning Principle

This is a dependency chain rather than seven isolated tutorials:

```text
Business problem
→ knowledge-access options
→ long context vs retrieval
→ trustworthy document representation
→ chunking when justified
→ retrieval mechanism
→ evidence assembly and evaluation
→ architecture recommendation
```

Later lessons depend on conclusions reached earlier: the access decision shapes document preparation, document representation shapes retrieval units, retrieval behavior shapes evidence assembly, and evaluation determines whether the final recommendation is defensible. Each day advances the same HealthSure decision rather than becoming an independent generic AI tutorial.

**Day numbers represent learning sequence, not mandatory calendar days.**

## Expected Artifacts

- Retrieval ADRs, an evaluation plan, a HealthSure knowledge-domain model, and the Week 1 architecture diagram.

## Exit Criteria

Explain why retrieval exists, when it is unnecessary, how indexing choices affect quality and cost, and what changes the recommendation.

[Full roadmap](../../ROADMAP.md) · [Bootcamp overview](../README.md)
