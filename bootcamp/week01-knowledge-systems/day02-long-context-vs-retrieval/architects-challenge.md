# Architect's Challenge — Day 2

HealthSure's hypothetical qualified-long-context pilot expands from one policy package to several products and jurisdictions. Each package can contain a base policy, amendments, benefit schedules, definitions, exclusions, and historical versions. Some questions are claim-specific; others are general policy questions. Authorization differs by agent role.

Measured evidence is incomplete. Long context appears simpler, while a filtered-retrieval prototype appears faster and uses fewer model-input tokens. Neither option has yet demonstrated acceptable production business outcomes or total cost.

Prepare a recommendation covering:

- known facts, assumptions, open questions, and hypotheses;
- business outcome and consequence of being wrong;
- simplest viable solution;
- long context, deterministic section assembly, and filtered-retrieval alternatives;
- deterministic applicability, package completeness, and authorization boundaries;
- strongest counterargument;
- model, retrieval, engineering, evaluation, review, and incident costs;
- ownership, monitoring, degradation, rollback, and manual review;
- security, privacy, lineage, and audit evidence;
- offline, held-out, adversarial, and pilot evaluation;
- release blockers and acceptable abstention;
- concrete triggers for adding, changing, or removing retrieval.

Start with:

> Which measured business or correctness limitation prevents the simpler qualified-context design from satisfying the requirement?

There is no single correct answer. Do not use the hypothetical scenario as a HealthSure fact.
