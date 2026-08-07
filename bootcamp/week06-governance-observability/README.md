# Week 6 — Governance, Security, Evaluation, and Observability

**Status:** Not Started

## Architectural Capability

Make enterprise AI trustworthy, measurable, and controllable.

## Why This Week Exists

Security, governance, evidence, and operability must shape system boundaries rather than be added after implementation.

## HealthSure Business Trigger

HealthSure must protect sensitive data, ground answers, authorize actions, monitor behavior, and investigate incidents.

## Major Topics

- Threat modeling, identity, authorization, prompt injection, sensitive-data controls, governance, evaluation, tracing, audit, red teaming, and incident response.

## Decisions to Make

- Where are authorization and policy controls enforced?
- Which evaluations block deployment, and what evidence is retained?
- How are model, retrieval, tool, data, and operating failures distinguished?

## Counterarguments to Explore

- Central governance may stop delivery.
- Evaluation scores may not predict outcomes.
- Observability can expose sensitive data, and guardrails can create false confidence.

## Expected Artifacts

- Threat model, authorization ADR, evaluation scorecard, observability architecture, incident runbook, and Week 6 update.

## Exit Criteria

Design layered controls, meaningful evidence, and operational ownership while explaining how governance enables safe delivery.

[Full roadmap](../../ROADMAP.md) · [Bootcamp overview](../README.md)
