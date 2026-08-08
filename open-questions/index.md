# Open Questions Index

| ID | Question | Current Position | Revisit Trigger | Status |
|---|---|---|---|---|
| HS-KA-001 | What question categories do service agents actually ask, and how frequently? | Unknown; must drive access-pattern selection. | Agent query analysis/user research available. | Open |
| HS-KA-002 | Which facts are governing policy knowledge versus member-specific operational state? | Treat as separate access classes until evidence shows otherwise. | Question taxonomy completed. | Open |
| HS-KA-003 | Which source is authoritative for policy, claims, benefit accumulators, provider/network state, and enrollment? | Never infer authority from convenience. | Source ownership confirmed. | Open |
| HS-KA-004 | Which policy version/effective-date rules determine the governing document for a request? | Correct versioning is a correctness boundary. | Legal/business semantics confirmed. | Open |
| HS-KA-005 | What freshness is required for each fact type? | No single global freshness target. | Product/risk requirements measured. | Open |
| HS-KA-006 | What latency is required for each question class? | Challenge global latency targets; connect them to measurable business outcomes. | Workflow/productivity evidence available. | Open |
| HS-KA-007 | What is the consequence of stale or incorrect information per question class? | Risk should determine fallback, abstention, and authoritative-read policy. | Risk classification completed. | Open |
| HS-KA-008 | What authorization differences exist across policy, member, and claim data? | Must be enforced at architectural boundaries. | Identity/access model confirmed. | Open |
| HS-KA-009 | What load and query limits exist on operational source systems? | Direct access remains viable only if sources can safely serve the workload. | Capacity measurements available. | Open |
| HS-KA-010 | How large and well structured is the policy corpus? | Needed before long context vs retrieval can be decided. | Corpus inventory completed. | Open |
| HS-KA-011 | How will knowledge access and answer quality be evaluated? | No architecture should be accepted without workload-relevant evidence. | Evaluation dataset and acceptance criteria defined. | Open |
| HS-LCR-001 | Can HealthSure deterministically resolve the applicable policy package by product, jurisdiction, service date, version, amendment, and authorization? | Required correctness boundary before either long context or retrieval. | Policy-domain rules and adversarial version tests available. | Open |
| HS-LCR-002 | Does qualified long context meet material-answer, limitation-recall, citation, abstention, and historical-version safety thresholds? | Experimental; no real HealthSure evaluation evidence exists yet. | Golden, held-out, adversarial, and pilot evaluations completed. | Open |
| HS-LCR-003 | What business outcome and end-to-end latency thresholds make the assistant valuable to agents? | Acceptance and latency are not sufficient without workflow outcomes. | Before/after research time, handle time, usefulness, and latency distributions measured. | Open |
| HS-LCR-004 | What is the cost per successful answer for qualified long context versus retrieval? | Compare complete TCO, not model tokens alone. | Real workload, token, retrieval, engineering, review, and incident costs measured. | Open |
| HS-LCR-005 | Which question categories may safely omit or default a service date? | Default to date-dependent unless a category is explicitly validated and allowlisted. | Policy-domain approval and regression evidence available. | Open |
| HS-LCR-006 | When would retrieval earn its additional ingestion, indexing, filtering, reranking, evaluation, and operational obligations? | Only after a measured long-context limit and a material net improvement are demonstrated. | Quality, latency, context-capacity, authorization, corpus-growth, or TCO threshold breached. | Open |
| HS-LCR-007 | Can in-flight deduplication reduce duplicate requests without unsafe completed-answer reuse? | Prefer immutable request identity; defer broad answer caching. | Pilot reuse, abandonment, notification, and wasted-cost measurements available. | Open |
