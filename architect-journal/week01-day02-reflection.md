# Architect's Reflection — Day 2

## What surprised me?

I began with the idea that long context was sufficient because the document could fit. The harder boundary is whether the system can establish the correct policy package, preserve all governing conditions, and prove that the model used them.

## Which assumption did I challenge?

I challenged the assumption that traffic and model-token cost alone should decide when retrieval is needed. Retrieval must improve the complete business outcome after ingestion, indexing, filtering, reranking, evaluation, operations, and failure risk are included.

## What would I still be uncomfortable defending?

I would be uncomfortable defending production use without verified historical applicability rules, package completeness, authorization, and evidence that the model does not overlook material exclusions. I would also be uncomfortable calling agent acceptance correctness.

## What evidence would I require before production approval?

Real question taxonomy and corpus evidence; policy-domain version and precedence rules; golden, held-out, and adversarial evaluations; independent answer review; material-error and abstention thresholds; research-time and handle-time outcomes; latency distributions; cost per successful answer; ownership, incident, audit, and rollback readiness.

## Could the design be simpler?

Yes. Use a bounded qualified-long-context pilot and regenerate from fresh evidence. Defer retrieval, completed-answer caching, and stable-component invalidation until recomputation or context limitations become measured business problems.
