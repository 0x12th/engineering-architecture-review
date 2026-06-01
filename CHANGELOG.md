# Changelog

All notable changes to this skill are documented in this file.

## 0.2.0 - 2026-06-01

### Added

- Architecture Decision Framework for challenging assumptions, comparing alternatives, validating problem existence, and applying confidence gates.
- Evidence Collection Rules for bounded quick scans, stop conditions, early findings, and finding quality.
- Prioritization and cost/benefit guidance that separates severity from what should be worked on next.

### Changed

- Design challenge mode now argues against weak proposals instead of only improving them.
- Quick scan mode now favors fewer high-value findings, disciplined exploration, and concise findings-only output.
- Review mode and implementation mode are now explicitly separated.

### Improved

- README installation flow now surfaces setup earlier and pins examples to `v0.2.0`.
- Examples now match the updated quick scan and design challenge output formats.

## 0.1.0 - 2026-05-31

### Added

- Initial `engineering-architecture-review` Zed Agent Skill.
- Architecture review workflow for quick scans, focused reviews, full reviews, implementation mode, and design challenge mode.
- Severity model: Critical, High, Medium, Low.
- Finding format covering problem, impact, root cause, proposed solution, complexity, risk, expected benefit, and evolution cost.
- Evolution-cost guidance covering operational cost, team cognitive load, migration cost, 1–3 year maintenance cost, and complexity delta.
- Organizational review guidance for ownership clarity, team autonomy, cross-team dependencies, and process problems disguised as architecture problems.
- Performance review guidance for CPU, memory, queues, network, database contention, resource utilization, and capacity limits.
- Migration review guidance for incremental rollout, old/new path coexistence, rollback, correctness validation, and observability.
- Communication rules to avoid filler commentary, exposed internal thinking, and unnecessary narration.
- Supporting templates for checklists, architecture review reports, improvement roadmaps, task breakdowns, and ADR drafts.
- Example outputs for quick scan, focused review, design challenge, and implementation mode.
