# Example: Design Challenge Mode

Prompt:

```text
Challenge this proposal: split the monolith into separate services for billing, notifications, and reporting. Focus on operational cost, migration risk, ownership, and long-term maintenance.
```

Example output:

```md
## What is likely correct

- Billing, notifications, and reporting likely have different change rates and reliability needs.
- Clearer boundaries could reduce accidental coupling if the current modules are tightly intertwined.
- Reporting may benefit from a separate read model if it creates database contention for transactional paths.

## Assumptions that may be wrong

- The proposal assumes deployment separation is required before proving module boundaries are stable.
- It does not show that one team can own each extracted service independently.
- It assumes three services reduce long-term complexity instead of moving complexity into deployment and operations.

## Missing evidence

- Current change frequency, incident history, deploy contention, and scaling pressure are not shown.
- Data ownership, migration order, dual writes, rollback, and correctness validation are not described.
- The new operational surface is not estimated: deploy pipelines, alerts, dashboards, runbooks, incidents, and capacity management.

## Alternatives considered

1. Current state: keep one deployable and accept the current coupling until measured pain is clear.
2. Minimal change: define internal module boundaries, ownership, and contract tests before changing deployment shape.
3. Proposed option: extract billing, notifications, and reporting into separate services.

## Cost comparison

- Current state cost: Unknown until change delay, reliability incidents, scaling limits, or ownership conflicts are measured.
- Minimal change cost: Low to medium; mostly refactoring, tests, documentation, and ownership clarification.
- Proposed option cost: High; requires migration, service operations, compatibility layers, observability, rollout, rollback, and long-term ownership.

## Risks

- Operational cost: Three services add at least three deployables, service-to-service failure modes, monitoring surfaces, and on-call paths.
- Migration risk: Billing extraction can create data consistency issues if transactions span monolith and service boundaries.
- Ownership risk: If the same team owns all services, extraction may increase coordination cost without improving autonomy.
- Performance risk: Reporting service calls may add network overhead unless reporting uses an intentional read model or async projection.
- Maintenance risk: Shared libraries or shared database tables can recreate monolith coupling with more operational complexity.

## Recommendation

Do not split all three services now. Stabilize module and data boundaries first, add observability around contention and change hotspots, then reassess whether one targeted extraction is justified.

## Confidence level

Medium. The recommendation should be revisited if production metrics show strong independent scaling needs or if ownership is already split across teams.
```
