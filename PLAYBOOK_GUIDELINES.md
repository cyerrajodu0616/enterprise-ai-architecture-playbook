# Playbook Guidelines

## Purpose

This document defines how every lesson, architecture decision, case-study update, cheat sheet, system design, and reflection in the Enterprise AI Architecture Playbook must be created and reviewed.

The methodology is designed to prevent technology-first learning and to develop architectural judgment.

## Daily Lesson Input

Each lesson begins with:

- a clearly named topic;
- a source tutorial, document, paper, or problem statement when available;
- the current HealthSure architecture state;
- and the learner’s open questions.

Source material is the primary basis of the lesson. Additional research may expand or challenge it, but source-derived content and external analysis must be clearly distinguished.

## Required Lesson Structure

### 1. Executive Summary

State:

- the business problem;
- the architectural tension;
- the recommendation under current assumptions;
- and the strongest counterargument.

### 2. Business Problem

Start with a business capability, not a technology.

Required questions:

- Who needs the capability?
- What outcome should improve?
- What is the cost of not solving it?
- What level of correctness, freshness, latency, reliability, and explainability is required?
- What is the consequence of being wrong?

### 3. Current State and Trigger

Explain:

- how the problem is handled today;
- what has changed;
- why the current design is no longer sufficient;
- and whether the trigger is growth, risk, cost, regulation, reliability, or a new capability.

### 4. Naive Explanation

Explain the core idea in plain language without removing the essential trade-off.

### 5. Requirements, Constraints, and Assumptions

Separate these explicitly.

- **Requirements** are outcomes the design must satisfy.
- **Constraints** are boundaries the team cannot freely change.
- **Assumptions** are beliefs that may be wrong.

For important assumptions, include confidence, evidence, impact if wrong, and a validation plan.

### 6. The Simplest Viable Solution

Before presenting sophisticated options, identify the simplest design that could satisfy the requirement.

Apply the Simplicity Test:

> What is the simplest solution that satisfies the business requirement?

Explain why that design may be sufficient and what limitation would force evolution.

### 7. Technical Deep Dive

Explain the mechanism deeply enough for a senior engineer to understand:

- data flow;
- state;
- interfaces;
- scaling behavior;
- consistency;
- failure modes;
- and operational implications.

Avoid API memorization unless it illustrates an architectural point.

### 8. Architecture Options

Present at least two credible options when alternatives exist.

For each option include:

- how it works;
- strengths;
- weaknesses;
- cost profile;
- operating model;
- security implications;
- scaling limits;
- lock-in;
- and suitable conditions.

Do not include fake alternatives merely to make the recommendation appear stronger.

### 9. Counterarguments and Devil’s Advocate

State the strongest argument against the preferred recommendation.

Required prompts:

- Why might we reject this design?
- What simpler option could work?
- What are we underestimating?
- Which assumption is fragile?
- What would operations challenge?
- What would security challenge?
- What would finance challenge?
- What would a startup choose differently?
- What would a regulated enterprise choose differently?

### 10. Trade-off Analysis

Discuss tensions rather than presenting unrelated benefits and drawbacks.

Common trade-offs include:

- quality versus cost;
- latency versus reliability;
- flexibility versus simplicity;
- centralization versus autonomy;
- consistency versus availability;
- convenience versus lock-in;
- delivery speed versus maintainability;
- automation versus human control.

### 11. Cost Model

Discuss total cost of ownership.

Relevant categories include:

- initial engineering;
- ongoing engineering;
- compute;
- storage;
- network;
- model usage;
- licenses;
- support;
- observability;
- security;
- compliance;
- evaluation;
- incidents;
- migration;
- lock-in;
- and opportunity cost.

When exact numbers are unavailable, define the variables and identify which variables dominate.

### 12. Operational Concerns

Every production recommendation must answer:

- Who owns it?
- How is it monitored?
- What are the service-level indicators?
- What fails first?
- How does it degrade?
- How is it deployed and rolled back?
- What is the recovery process?
- What data must be replayed or rebuilt?
- What requires on-call support?
- What runbooks are needed?
- How is capacity planned?

### 13. Security, Privacy, and Governance

Discuss when relevant:

- identity;
- authentication;
- authorization;
- least privilege;
- data classification;
- sensitive-data handling;
- tenant isolation;
- auditability;
- lineage;
- retention;
- data residency;
- human approval;
- regulatory evidence;
- model and prompt governance;
- third-party risk.

### 14. Evaluation and Evidence

Define how the recommendation will be validated.

Include:

- offline evaluation;
- online evaluation;
- golden datasets;
- business metrics;
- technical metrics;
- failure thresholds;
- experiment duration;
- acceptance criteria;
- and rollback criteria.

Do not use a generic benchmark as the only evidence.

### 15. Final Recommendation

Use this form:

> Under these assumptions and constraints, we recommend **X** because **Y**. We accept **trade-offs A and B** to gain **benefits C and D**. We would not recommend this approach when **conditions E and F** apply.

### 16. What Would Change the Recommendation?

List concrete triggers such as:

- scale threshold;
- cost threshold;
- latency requirement;
- regulatory change;
- model capability change;
- team maturity;
- quality regression;
- vendor risk;
- new data source;
- or new business use case.

### 17. HealthSure Case-Study Update

Document:

- new business requirement;
- previous architecture;
- decision made;
- components added, changed, or removed;
- new risks;
- new metrics;
- new open questions;
- and the next expected pressure on the design.

### 18. Architect’s Challenge

End with a realistic scenario that has no single correct answer.

The response must state:

- assumptions;
- recommendation;
- alternatives;
- counterargument;
- cost;
- operations;
- and change triggers.

### 19. Architect’s Reflection

Answer:

1. What surprised me?
2. Which assumption did I challenge?
3. What would I still be uncomfortable defending?
4. What evidence would I require before production approval?
5. Could the design be simpler?

## Required Artifacts

A complete lesson produces:

1. Detailed HTML lesson page.
2. Architecture Decision Record.
3. Five-minute interview cheat sheet.
4. HealthSure case-study update.
5. Architect’s Challenge.
6. Open questions.
7. Architect’s Reflection.

A lesson may also produce source code, cost calculators, data models, system diagrams, evaluation datasets, experiment plans, or runbooks.

## ADR Standard

Each ADR must include:

- title;
- status;
- date;
- owners;
- business context;
- decision scope;
- assumptions;
- constraints;
- options considered;
- decision drivers;
- recommendation;
- strongest counterargument;
- trade-offs accepted;
- consequences;
- cost implications;
- operational impact;
- security and governance impact;
- validation plan;
- review triggers;
- and superseded decisions.

ADR statuses:

- Proposed
- Accepted
- Experimental
- Deprecated
- Superseded
- Rejected

## HTML Lesson Standard

Each HTML page must:

- have a descriptive title;
- state the business problem early;
- include the current recommendation;
- use diagrams where they improve understanding;
- include alternatives and counterarguments;
- include cost and operations;
- avoid unsupported universal claims;
- provide navigation to previous and next lessons;
- be readable on desktop and mobile;
- and avoid coupling content to one vendor unless explicitly vendor-specific.

## Cheat-Sheet Standard

The cheat sheet should be reviewable in five minutes and contain:

- one-sentence definition;
- business problem;
- when to use;
- when not to use;
- key trade-offs;
- common failure modes;
- cost drivers;
- operational concerns;
- strongest counterargument;
- interview framing;
- and what changes the recommendation.

## Evidence and Citation Standard

Distinguish among:

- source-derived facts;
- measured project evidence;
- external research;
- architectural inference;
- and opinion.

Use primary sources for technical claims when practical.

Time-sensitive claims must include a date or source.

Never present an unverified estimate as a fact.

## Writing Standard

The writing should be professional, calm, evidence-driven, direct, and accessible to experienced engineers.

Avoid:

- unexplained jargon;
- absolute recommendations;
- technology worship;
- fake precision;
- generic benefits lists;
- and claims that ignore operations or cost.

Prefer phrases such as:

- “under these assumptions”;
- “the trade-off is”;
- “the simpler alternative is”;
- “the dominant cost driver is”;
- “the first failure mode is”;
- “we would change the decision when”.

## Review Workflow

Documents pass through:

1. Draft
2. Technical Review
3. Architecture Review
4. Approved
5. Published
6. Revisited

A document is not approved merely because it is complete. It must pass the quality gates.

## Quality Gates

### Accuracy

Are technical claims defensible?

### Completeness

Are business, cost, operations, security, governance, alternatives, and counterarguments covered where relevant?

### Clarity

Can a senior engineer understand the reasoning without additional explanation?

### Practicality

Could a real team use the guidance?

### Simplicity

Was the simplest viable solution evaluated first?

### Evidence

Are facts, assumptions, estimates, and opinions distinguishable?

### Timelessness

Does the document focus on durable reasoning rather than current hype?

### Adaptability

Does it explain when the recommendation should change?

## Definition of Done

A lesson is complete only when the learner can answer:

- Why does this capability exist?
- What business problem does it solve?
- What is the simplest viable solution?
- What alternatives exist?
- Why might the recommendation be wrong?
- What assumptions make it valid?
- What is the dominant cost?
- What breaks first?
- Who operates it?
- How is it secured and governed?
- How is success measured?
- How would the recommendation change?
- How would the decision be explained to a CTO?

## Daily Prompt for a New Lesson Chat

```text
We are continuing the Enterprise AI Architecture Playbook.

Today's topic is: <TOPIC>

Primary source:
<ATTACH OR IDENTIFY THE SOURCE>

Teach this as if you are mentoring a Senior Data Engineer becoming an AI/Data Architect.

Optimize for architectural judgment, not speed.

Use the repository's ARCHITECTURE_MANIFESTO.md, PLAYBOOK_GUIDELINES.md, ROADMAP.md, current ADRs, and HealthSure case-study state as the governing context.

Every recommendation must include:
- Business problem
- Requirements, constraints, and assumptions
- Simplest viable solution
- Alternatives
- Strongest counterargument
- Trade-offs
- Cost implications
- Operational concerns
- Security and governance
- Evaluation and evidence
- Final recommendation
- What would change the recommendation

End with:
- HTML page
- ADR
- Five-minute cheat sheet
- HealthSure update
- Architect's Challenge
- Open questions
- Architect's Reflection
```

## Governing Principle

> Every statement should answer **Why?** before it answers **How?**
