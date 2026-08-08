# Five-Minute Cheat Sheet — Long Context vs Retrieval

## One-sentence definition

Long context supplies a qualified body of evidence directly; retrieval operates an additional selection subsystem to send a smaller evidence set.

## Business problem

Give agents correct, useful, traceable policy explanations without allowing inapplicable versions, missing clauses, excessive latency, or unjustified infrastructure to undermine the outcome.

## Simplicity rule

Start with deterministically qualified context when the governing package fits and passes evaluation. Add retrieval only when a measured limitation earns its obligations.

## Applicability boundary

Resolve product, jurisdiction, service date, policy version, amendments, schedules, package completeness, and authorization before either long context or retrieval.

## When to use long context

- bounded qualified package;
- surrounding meaning matters;
- omission risk dominates;
- latency and unit economics satisfy requirements;
- team should avoid unproven retrieval operations.

## When not to use it

- context capacity is exceeded;
- model repeatedly overlooks governing evidence;
- authorization requires finer selection;
- measured latency or cost damages the business outcome;
- cross-corpus questions make complete context impractical.

## Failure taxonomy

- wrong policy package selected;
- required evidence omitted;
- evidence present but overlooked;
- precedence interpreted incorrectly;
- unsupported conclusion;
- unsafe answer instead of abstention.

## Key trade-offs

- completeness versus focused attention;
- model cost versus retrieval TCO;
- simple pilot versus unproven future scale;
- safe abstention versus operational usefulness.

## Cost drivers

Long context: request volume, input tokens, follow-up turns, model price, evaluation, review, and incidents.

Retrieval: ingestion, embeddings/indexing, storage, reprocessing, query/reranking, model context, engineering, on-call, evaluation, and incidents.

Compare cost per correct, useful answer—not cost per call.

## Operations and governance

- immutable claim-scoped request context;
- authorization revalidation;
- package manifest and lineage;
- release blockers and abstention;
- audit evidence separate from cache;
- rollback and affected-question disablement.

## Strongest counterargument

Long context may be operationally simpler while economically and cognitively inefficient. Retrieval may focus evidence and lower latency, but it can silently omit the clause that changes the answer.

## Interview framing

> I would not choose retrieval because the corpus is large or long context because it fits. I would first establish applicability and authorization, test the simplest qualified-context design on the real workload, classify failures by business consequence, compare full TCO, and add retrieval only when it demonstrates material net value.

## What changes the recommendation

Material errors, historical-version contamination, package omissions, context limits, latency, cost per successful answer, authorization granularity, corpus growth, or retrieval evidence that improves the complete business outcome.
