# Week 4 — Models, Context, Tools, and Workflows

**Status:** Not Started

## Architectural Capability

Choose the correct execution mechanism for each AI task.

## Why This Week Exists

Not every requirement is a prompting problem; deterministic code, retrieval, SQL, APIs, workflows, or human review may be safer and simpler.

## HealthSure Business Trigger

The assistant must answer questions, calculate coverage implications, look up claim status, compare policy versions, and initiate approved workflows.

## Major Topics

- Model selection, context engineering, structured outputs, tools, APIs, SQL, caching, routing, fine-tuning, workflows, and human approval.

## Decisions to Make

- When should HealthSure use evidence, authoritative tools, deterministic logic, or model reasoning?
- Which models are sufficient for each workload?
- Which actions require human approval and failure containment?

## Counterarguments to Explore

- Tool calling expands integration and security risk.
- Fine-tuning and abstraction may add lifecycle cost without value.
- Caching can return stale or unauthorized information.

## Expected Artifacts

- Model-routing and execution ADRs, a task decision tree, failure-mode analysis, and Week 4 architecture update.

## Exit Criteria

Select among context, retrieval, data access, deterministic workflows, model reasoning, and human review using business risk and operational evidence.

[Full roadmap](../../ROADMAP.md) · [Bootcamp overview](../README.md)
