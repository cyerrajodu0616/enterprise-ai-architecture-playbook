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
