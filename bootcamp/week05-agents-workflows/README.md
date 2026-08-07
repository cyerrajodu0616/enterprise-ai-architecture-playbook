# Week 5 — Agents and Business Workflows

**Status:** Not Started

## Architectural Capability

Determine when autonomous or semi-autonomous behavior is justified and how to bound it.

## Why This Week Exists

Agents introduce variable behavior, permissions, state, and recovery obligations that must earn their place over simpler workflows.

## HealthSure Business Trigger

HealthSure wants to automate claim-document collection, policy research, case summarization, and selected operational follow-ups.

## Major Topics

- Agents versus workflows, planning, state, memory, permissions, idempotency, retries, human approval, audit, replay, and evaluation.

## Decisions to Make

- Does the work require dynamic planning?
- Which steps and permissions must remain deterministic or human-approved?
- How are state, retries, duplicated actions, audit, replay, and shutdown handled?

## Counterarguments to Explore

- A workflow or state machine may be more reliable and transparent.
- Memory can create privacy and correctness risks.
- Autonomous action may be unacceptable in regulated processes.

## Expected Artifacts

- Agent-versus-workflow ADR, permission model, state-machine diagram, approval policy, evaluation plan, and Week 5 update.

## Exit Criteria

Reject unnecessary agents and define safe state, permission, idempotency, audit, recovery, and oversight boundaries for justified ones.

[Full roadmap](../../ROADMAP.md) · [Bootcamp overview](../README.md)
