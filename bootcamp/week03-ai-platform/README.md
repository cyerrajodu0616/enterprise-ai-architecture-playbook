# Week 3 — Enterprise AI Platform

**Status:** Not Started

## Architectural Capability

Define shared enterprise AI capabilities and the boundaries between platform and product teams.

## Why This Week Exists

A platform can reduce duplication and improve governance, but excessive centralization can become a bottleneck.

## HealthSure Business Trigger

Policy service, claims, underwriting, legal, and provider teams want distinct assistants with shared enterprise controls.

## Major Topics

- Platform product thinking, model gateways, provider abstraction, shared retrieval, tenant isolation, APIs, build versus buy, and developer experience.

## Decisions to Make

- Which capabilities should be standardized or domain-owned?
- Should applications share model and retrieval gateways?
- How should providers, quotas, cost allocation, and onboarding work?

## Counterarguments to Explore

- Central platforms can delay product teams.
- Abstractions can hide material provider differences.
- Shared services may force unsuitable designs and deepen lock-in.

## Expected Artifacts

- Platform-boundary ADRs, a capability map, responsibility matrix, API boundary diagram, and Week 3 architecture update.

## Exit Criteria

Define and defend a platform boundary that balances reuse, governance, cost, team autonomy, and operating ownership.

[Full roadmap](../../ROADMAP.md) · [Bootcamp overview](../README.md)
