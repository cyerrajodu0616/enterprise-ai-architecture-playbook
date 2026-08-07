# Two-Month Roadmap

## Developing Architectural Judgment for Enterprise AI Systems

## Purpose

This roadmap is not a list of technologies to memorize. It is a sequence of architectural capabilities.

The order mirrors how an enterprise AI platform should be reasoned about:

1. understand the business and decision framework;
2. determine how knowledge and data should be accessed;
3. build reliable AI-ready data foundations;
4. define platform boundaries;
5. introduce automation only when justified;
6. secure, govern, observe, and evaluate the system;
7. design for scale, resilience, and economics;
8. integrate the decisions into a coherent enterprise architecture.

Each week evolves the HealthSure case study and ends with an architecture review.

## Program Operating Model

- **Duration:** 8 weeks
- **Cadence:** 5 focused lessons plus 1 review/consolidation session per week
- **Primary case study:** HealthSure Insurance
- **Secondary domains:** retail, banking, healthcare, manufacturing, public sector, and startup environments
- **Primary outputs:** HTML lessons, ADRs, cheat sheets, case-study updates, decision trees, cost models, and system designs

The roadmap is directional rather than rigid. A topic may take more than one day when deeper exploration is needed.

Depth is preferred over topic count.

# Week 1 — Knowledge Access and Grounded Answers

## Capability

Design how an AI system finds and uses enterprise knowledge.

## Why this comes first

Many enterprise AI initiatives begin as knowledge assistants. This makes knowledge access a practical entry point, but the goal is not to assume retrieval is always required.

The core habit is:

> Choose among direct context, structured access, search, retrieval, and tool use based on the business problem.

## HealthSure trigger

HealthSure wants an internal assistant that helps service agents answer policy questions accurately and cite governing policy language.

The initial corpus is small, but the business expects rapid growth.

## Topics

- Why grounding exists
- Long context versus retrieval versus structured access
- Document ingestion and parsing
- Chunking as an architectural decision
- Dense, sparse, and hybrid retrieval
- Metadata filtering and access control
- Parent-child and structure-aware retrieval
- Reranking and context assembly
- Retrieval evaluation
- When not to use retrieval

## Decisions

- Should HealthSure send complete documents or retrieve passages?
- When is keyword search sufficient?
- When should the system call a structured API or database instead of searching text?
- How should chunk size be selected from answer-span and cost evidence?
- Which metadata belongs in the retrieval layer?
- How should versioned policy language be retrieved for the date of a claim?
- What quality threshold is required before deployment?

## Counterarguments

- Long-context models may remove some retrieval complexity.
- Retrieval can omit relevant evidence.
- Chunking can destroy meaning.
- A vector database may be unnecessary.
- Structured systems may be more authoritative than documents.
- Added retrieval infrastructure may cost more to operate than it saves.

## Artifacts

- ADR: Long Context vs Retrieval vs Structured Access
- ADR: Initial Chunking Strategy
- Retrieval evaluation plan
- HealthSure knowledge-domain model
- Cost comparison
- Week 1 architecture diagram

## Exit criteria

The learner can explain why retrieval exists, when it is unnecessary, how chunking affects quality and cost, how metadata affects correctness and scale, how to evaluate retrieval, and how the recommendation changes as corpus size, traffic, or model cost changes.

# Week 2 — AI-Ready Data Foundations

## Capability

Design the data lifecycle that makes enterprise AI reliable, fresh, traceable, and reproducible.

## Why this comes next

AI quality depends on the quality, history, ownership, and accessibility of enterprise data.

Before designing a shared AI platform, the underlying data lifecycle must be understood.

## HealthSure trigger

Policy documents, claims data, provider data, and operational procedures are distributed across multiple systems. Updates arrive at different frequencies and have different legal meanings.

## Topics

- Source-system classification
- Batch versus streaming
- Change data capture
- Data contracts and schema evolution
- Lake, warehouse, and lakehouse responsibilities
- Open table formats and versioned datasets
- Raw, validated, curated, and serving layers
- Metadata, catalog, lineage, and ownership
- Data quality for AI systems
- Bitemporal and effective-dated data
- Unstructured and structured data integration
- Reprocessing and reproducibility

## Decisions

- Which data requires real-time updates?
- Which data is adequately served through batch?
- Where should canonical enterprise data live?
- How are document versions related to policy effective dates?
- How are changed records reprocessed?
- How can an answer be reproduced months later?
- Which transformations are centralized, and which belong to domain teams?

## Simplicity Test

If the business consumes the result once per day, does streaming create measurable additional value?

## Counterarguments

- A lakehouse may duplicate existing warehouse capabilities.
- Streaming can introduce unnecessary operational burden.
- Centralized data models can slow domain teams.
- Layering can become ceremony without measurable value.
- A metadata catalog is useless when ownership processes are absent.

## Artifacts

- ADR: Batch vs Streaming for Policy Updates
- ADR: System of Record and Canonical Data Boundaries
- HealthSure data-domain map
- Data lineage diagram
- Data-quality SLOs
- Week 2 architecture update

## Exit criteria

The learner can decide batch versus streaming, define warehouse and lakehouse responsibilities, model versioned enterprise data, make AI datasets reproducible, and explain how data ownership affects platform design.

# Week 3 — Enterprise AI Platform Boundaries

## Capability

Define which capabilities should be centralized as a platform and which should remain within product or domain teams.

## Why this matters

An AI platform can reduce duplication, improve governance, and accelerate delivery. It can also become a centralized bottleneck that forces unrelated workloads into one design.

## HealthSure trigger

Multiple teams want to build assistants for policy service, claims operations, underwriting, legal research, and provider support.

## Topics

- Platform product thinking
- Shared versus domain-owned capabilities
- Model gateway responsibilities
- Model abstraction and routing
- Prompt and configuration management
- Embedding services
- Retrieval services
- Tool and API integration
- Tenant and domain isolation
- Platform APIs and contracts
- Build versus buy
- Internal developer experience

## Decisions

- Which capabilities must be standardized?
- Where should product teams retain autonomy?
- Should every application use one model gateway?
- How should model providers be replaceable?
- Is one shared retrieval service appropriate?
- How are cost and quotas allocated?
- How do teams onboard without waiting on the platform team?

## Counterarguments

- Central platforms can become bottlenecks.
- Abstraction can hide important provider differences.
- A common service may force the wrong design on specialized workloads.
- Build-versus-buy decisions can create expensive lock-in.
- Platform teams can optimize for control instead of developer outcomes.

## Artifacts

- ADR: Centralized Model Gateway
- ADR: Shared vs Domain-Owned Retrieval
- Platform capability map
- Responsibility matrix
- API boundary diagram
- Week 3 architecture update

## Exit criteria

The learner can define a platform boundary, defend centralization and decentralization decisions, and identify how governance, cost allocation, and team autonomy interact.

# Week 4 — Model, Context, Tool, and Workflow Decisions

## Capability

Choose the correct mechanism for each AI task rather than treating every requirement as a prompting problem.

## HealthSure trigger

The business wants the assistant to answer questions, calculate coverage implications, look up claim status, compare policy versions, and initiate approved workflows.

## Topics

- Model selection by workload
- Context engineering
- Structured outputs
- Tool calling
- SQL and API access
- Retrieval plus tools
- Prompt caching and semantic caching
- Model routing
- Fine-tuning and adaptation
- Deterministic workflow versus model reasoning
- Human-in-the-loop
- Failure containment

## Decisions

- When should the model answer from supplied evidence?
- When should it call an authoritative system?
- When is deterministic code better than model reasoning?
- When is a smaller model sufficient?
- When does fine-tuning justify its lifecycle cost?
- Which actions require human approval?
- How are partial failures handled?

## Simplicity Test

Could a deterministic rule, SQL query, or API call solve the problem more safely than a model?

## Counterarguments

- Tool calling creates integration and security risk.
- Model abstraction may reduce quality.
- Fine-tuning may be unnecessary.
- Caching can return stale or unauthorized information.
- Human approval can remove the expected productivity benefit.

## Artifacts

- ADR: Model Selection and Routing
- ADR: Tool Call vs Retrieval vs Direct Answer
- Decision tree for task execution
- Failure-mode analysis
- Week 4 architecture update

## Exit criteria

The learner can select among context, retrieval, SQL, APIs, deterministic workflows, model reasoning, and human review based on business risk and operational constraints.

# Week 5 — Agents and Business Process Automation

## Capability

Determine when autonomous or semi-autonomous behavior is justified and how to bound it.

## HealthSure trigger

HealthSure wants to automate claim-document collection, policy research, case summarization, and selected operational follow-ups.

## Topics

- What qualifies as an agent
- Deterministic workflow versus agent
- Planning and execution
- State and memory
- Tool permissions
- Idempotency and retries
- Human approval
- Multi-agent claims
- Workflow engines
- Long-running processes
- Audit and replay
- Agent evaluation

## Decisions

- Does the workflow require dynamic planning?
- Which steps must remain deterministic?
- What actions may be taken without approval?
- Where is state stored?
- How are retries prevented from duplicating business actions?
- How is every action audited and replayed?
- How is an agent stopped?

## Counterarguments

- Agents may be less reliable than workflows.
- Multi-agent designs may add communication overhead without value.
- Memory can introduce privacy and correctness problems.
- Autonomous action may be unacceptable in regulated processes.
- Agent frameworks can obscure simple state machines.

## Artifacts

- ADR: Agent vs Deterministic Workflow
- Permission model
- State-machine diagram
- Human-approval policy
- Agent evaluation plan
- Week 5 architecture update

## Exit criteria

The learner can reject unnecessary agents, define safe boundaries for justified agents, and explain state, permissions, idempotency, audit, and human oversight.

# Week 6 — Governance, Security, Evaluation, and Observability

## Capability

Make enterprise AI trustworthy, measurable, and controllable.

## HealthSure trigger

HealthSure must demonstrate that sensitive data is protected, answers are grounded, actions are authorized, behavior is monitored, and incidents can be investigated.

## Topics

- AI threat modeling
- Identity and authorization
- Prompt injection and tool abuse
- Sensitive-data controls
- Tenant isolation
- Model and data governance
- Evaluation architecture
- Online and offline metrics
- Tracing and observability
- Auditability
- Policy enforcement
- Red teaming
- Incident response
- Responsible human oversight

## Decisions

- Where are authorization checks enforced?
- How are prompts and tools protected from untrusted content?
- How is sensitive data prevented from crossing boundaries?
- Which evaluations block deployment?
- What evidence is retained for audits?
- How are model, prompt, retrieval, and tool failures distinguished?
- Who can approve high-risk changes?

## Counterarguments

- Excessive central governance can stop delivery.
- Evaluation scores may not predict business outcomes.
- Observability can expose sensitive data.
- Guardrails can create false confidence.
- Human review can become an unscalable bottleneck.

## Artifacts

- AI threat model
- ADR: Authorization Enforcement Boundary
- Evaluation scorecard
- Observability architecture
- Incident runbook
- Week 6 architecture update

## Exit criteria

The learner can design layered controls, distinguish evaluation from monitoring, define meaningful metrics, and explain how governance enables rather than merely restricts delivery.

# Week 7 — Scale, Reliability, Performance, and Economics

## Capability

Design an AI platform that remains reliable and economically sustainable as usage grows.

## HealthSure trigger

The platform expands from internal pilots to enterprise-wide use, with customer-facing workloads, strict service levels, and multiple regions.

## Topics

- Capacity modeling
- Latency budgets
- Cost modeling and unit economics
- Caching strategies
- Rate limits and quotas
- Backpressure and load shedding
- Availability targets
- Failure isolation
- Multi-region architecture
- Disaster recovery
- Reindexing and migration
- Vendor outages
- Performance testing
- FinOps for AI

## Decisions

- What is the cost per successful business outcome?
- Which workloads deserve premium models?
- Where should caching occur?
- How does the platform degrade under load?
- What availability is worth paying for?
- Which data and services require multi-region replication?
- How can a provider be replaced during an outage?
- What recovery-time and recovery-point objectives are justified?

## Counterarguments

- Multi-region can cost more than the business impact of downtime.
- Caching may compromise freshness or authorization.
- Redundancy can multiply consistency and operating complexity.
- Aggressive optimization may reduce quality.
- Cost allocation can discourage useful experimentation.

## Artifacts

- Unit-economics model
- ADR: Multi-Region Strategy
- ADR: Model Routing by Cost and Risk
- Capacity plan
- Degradation strategy
- Disaster-recovery design
- Week 7 architecture update

## Exit criteria

The learner can create a capacity and cost model, define appropriate service levels, design graceful degradation, and distinguish justified resilience from expensive overengineering.

# Week 8 — Integrated Enterprise Architecture and Cross-Domain Adaptation

## Capability

Integrate previous decisions into a coherent architecture and adapt the reasoning to unfamiliar businesses.

## HealthSure trigger

The architecture must be presented for executive approval, security review, operational readiness, and phased investment.

## Topics

- End-to-end architecture synthesis
- Capability and domain mapping
- Migration strategy
- Architecture runway
- Executive communication
- Decision sequencing
- Technical-debt management
- Build-versus-buy portfolio review
- Cross-domain adaptation
- Architecture review board simulation
- Portfolio publication
- Future roadmap

## Final review questions

- Which HealthSure decisions are still valid?
- Which assumptions changed?
- Which components can be removed?
- What is the largest unmitigated risk?
- What is the dominant cost?
- Which decision is hardest to reverse?
- What should be built now, deferred, or rejected?
- How would the architecture differ for a startup?
- How would it differ for retail, banking, healthcare, or manufacturing?
- How would we explain the design to a CTO in ten minutes?

## Artifacts

- Final HealthSure architecture
- Architecture narrative
- Executive decision memo
- Consolidated ADR index
- Risk register
- Cost model
- Migration roadmap
- Cross-domain comparison
- Portfolio publication plan

## Exit criteria

The learner can receive an unfamiliar enterprise AI problem and:

1. clarify the business outcome;
2. identify requirements, constraints, and assumptions;
3. propose the simplest viable design;
4. compare credible alternatives;
5. present counterarguments;
6. analyze cost, operations, security, governance, and failure;
7. recommend a phased architecture;
8. identify evidence and review triggers;
9. explain the decision to technical and executive audiences.

# Weekly Architecture Review

At the end of each week, return to the Home Base session and review:

- What did we learn?
- Which assumption changed?
- Which ADR remains valid?
- Which ADR needs revision?
- What complexity did we introduce?
- Could anything be removed?
- What new risk appeared?
- What is now the dominant cost?
- What should the next week prioritize?

# Questions We Should Be Able to Answer by the End

## Business and simplicity

- What is the business outcome?
- What is the simplest design that satisfies it?
- Which complexity is necessary?
- Which complexity is optional?
- What would the customer notice if removed?

## Data

- When is batch better than real-time?
- When is streaming justified?
- Where should canonical data live?
- How should versioned and effective-dated data be modeled?
- How can an answer be reproduced later?

## Knowledge and grounding

- When is long context sufficient?
- When is retrieval required?
- When is keyword search better than semantic search?
- When should the model use SQL or an API instead of documents?
- How should retrieval quality be evaluated?

## Platform

- Which capabilities should be centralized?
- Which capabilities should remain domain-owned?
- How should models and vendors be replaceable?
- How should cost and quotas be allocated?
- How can a platform avoid becoming a bottleneck?

## Models and workflows

- When is a smaller model sufficient?
- When is fine-tuning justified?
- When is deterministic code better than model reasoning?
- When should a human approve an action?
- When is an agent worse than a workflow?

## Governance and operations

- Where is authorization enforced?
- How is sensitive data protected?
- How is behavior evaluated and monitored?
- What happens when the model, retrieval layer, or tool fails?
- Who owns the incident?

## Scale and economics

- What is the unit cost per successful outcome?
- Which cost dominates?
- What breaks first?
- How does the platform degrade gracefully?
- When does multi-region resilience justify its cost?

## Evolution

- Which decisions are reversible?
- Which decisions are expensive to reverse?
- What evidence would change the recommendation?
- How does the design adapt when capability or price changes?
- Which components should disappear as the system matures?

# Program Principle

The roadmap is complete only when it improves judgment.

Completing a topic without being able to defend its assumptions, alternatives, counterarguments, cost, operations, and evolution is not progress.

> **Why before How. Simplicity before Sophistication. Evidence before Confidence.**
