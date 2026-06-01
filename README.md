# Engineering Architecture Review

Engineering Architecture Review is an AI skill for practical architecture reviews. It helps coding agents inspect real systems, challenge weak proposals, and recommend incremental changes based on evidence, confidence, operational cost, migration risk, ownership, reliability, performance, and long-term maintenance.

Use it when you want:

- a short architecture scan with the highest-value findings;
- a focused review of one subsystem, migration, deployment path, or reliability concern;
- a design challenge that tests whether a proposed architecture change should exist at all;
- a small implementation follow-up after findings are already established.

## Installation

### Zed: import from URL

In Zed, use `agent: create skill from url` and provide a GitHub Markdown URL to the root `SKILL.md`.

Latest:

```text
https://raw.githubusercontent.com/0x12th/engineering-architecture-review/master/SKILL.md
```

Pinned version:

```text
https://raw.githubusercontent.com/0x12th/engineering-architecture-review/v0.2.0/SKILL.md
```

This imports `SKILL.md`. If you also want `templates/` and `examples/`, use the clone-based installation below.

### Install latest with templates

```sh
git clone https://github.com/0x12th/engineering-architecture-review.git
mkdir -p ~/.agents/skills
cp -R engineering-architecture-review ~/.agents/skills/engineering-architecture-review
```

### Install pinned version with templates

```sh
git clone --branch v0.2.0 --depth 1 https://github.com/0x12th/engineering-architecture-review.git
mkdir -p ~/.agents/skills
cp -R engineering-architecture-review ~/.agents/skills/engineering-architecture-review
```

### Claude Code

```sh
git clone --branch v0.2.0 --depth 1 https://github.com/0x12th/engineering-architecture-review.git
mkdir -p ~/.claude/skills
cp -R engineering-architecture-review ~/.claude/skills/engineering-architecture-review
```

If Claude Code does not auto-load the skill, reference it from `CLAUDE.md` or your prompt:

```text
For architecture review tasks, follow the instructions in .claude/skills/engineering-architecture-review/SKILL.md.
```

### Project-local installation

For Zed:

```sh
git clone --branch v0.2.0 --depth 1 https://github.com/0x12th/engineering-architecture-review.git
mkdir -p <project>/.agents/skills
cp -R engineering-architecture-review <project>/.agents/skills/engineering-architecture-review
```

For Claude Code:

```sh
git clone --branch v0.2.0 --depth 1 https://github.com/0x12th/engineering-architecture-review.git
mkdir -p <project>/.claude/skills
cp -R engineering-architecture-review <project>/.claude/skills/engineering-architecture-review
```

## Why This Exists

Typical AI architecture reviews often optimize for architectural vocabulary instead of engineering outcomes. They may suggest service extraction, rewrites, new layers, or abstract patterns without proving that the change improves delivery speed, reliability, operations, or maintenance cost.

This skill pushes the agent to inspect the system first, state uncertainty, separate important risks from cosmetic issues, and recommend changes that can be shipped safely.

## Philosophy

The goal is not to create the cleanest architecture.

The goal is to reduce long-term maintenance cost while preserving delivery speed, operational stability, and migration safety.

## Unlike Typical AI Architecture Reviews

This skill explicitly evaluates the following before recommending architectural changes:

- operational cost
- migration cost
- team cognitive load
- ownership boundaries
- performance and capacity impact
- long-term maintenance cost over 1–3 years

The default bias is not “make it cleaner.” The default bias is: reduce future maintenance cost without increasing operational risk.

## Quick Start

Use one of these prompts after installing the skill.

### Quick scan

```text
Use engineering-architecture-review.

Run a quick architecture scan of this repository.
Return only the top 5 findings.
```

### Focused review

```text
Use engineering-architecture-review.

Review the worker queue architecture for reliability, observability, and performance bottlenecks.
Keep the review focused.
```

### Design challenge

```text
Use engineering-architecture-review.

Challenge this proposal: split the monolith into separate services for billing, notifications, and reporting.
Focus on operational cost, migration risk, ownership, and long-term maintenance.
```

### Implementation mode

```text
Use engineering-architecture-review in implementation mode.

Make the smallest safe change to improve observability for this migration path.
Validate what you change.
```

## What You Get

The skill produces findings like:

- Hidden coupling between modules
- Missing ownership boundaries
- Migration risks
- Reliability gaps
- Observability blind spots
- Performance and capacity bottlenecks
- Unnecessary architectural complexity

Each finding includes:

- severity
- impact
- evidence or uncertainty
- root cause
- minimal fix
- expected benefit

## Example Finding

Project:
Telegram bot for downloading Instagram reels.

Finding:
Global concurrency limit missing.

Severity:
High

Impact:
Multiple users can trigger parallel downloads and exhaust CPU, network bandwidth, or disk I/O.

Evidence:
Download requests start work directly, and no global queue, semaphore, or saturation metric is visible.

Root cause:
Concurrency is controlled per request path instead of through a shared resource limit.

Minimal fix:
Introduce a global semaphore and expose queue saturation metrics.

Expected benefit:
Predictable resource usage under load.

## Example Output

Quick scan output is intentionally short:

```md
1. Global concurrency limit missing
   Severity: High
   Impact: Under load, worker fan-out can overload the database or external provider and cause cascading failures.
   Evidence: Worker configuration defines per-process concurrency, but no global limit or queue-age metric is visible.
   Minimal fix: Add a bounded concurrency limit and expose saturation metrics before increasing worker count.

2. Health checks do not verify Redis
   Severity: Medium
   Impact: The service can report healthy while request paths depending on Redis are failing.
   Evidence: Health endpoint only verifies process liveness and database connectivity.
   Minimal fix: Add a readiness check for Redis-dependent paths or expose degraded readiness for cache/session failures.

3. Schema migration rollback is not defined
   Severity: High
   Impact: A failed deploy can leave old and new code incompatible with the database schema.
   Evidence: Migration files exist, but deployment docs do not describe compatibility, rollback, or verification.
   Minimal fix: Use an expand/contract migration plan and document rollback or roll-forward steps.
```

More examples:

- `examples/quick-scan.md`
- `examples/focused-review.md`
- `examples/design-challenge.md`
- `examples/implementation-mode.md`

## What It Reviews

The skill guides an agent to review systems across these areas:

- Modularity, cohesion, coupling, dependency direction, and cyclic dependencies
- Architecture debt, unclear ownership, weak abstractions, and misplaced responsibilities
- Service boundaries, API contracts, data ownership, and migration safety
- Observability: logs, metrics, traces, alerts, dashboards, health checks, and runbooks
- Reliability: failure modes, retries, timeouts, idempotency, backpressure, and rollback
- Deployment readiness: configuration, migrations, resource limits, release safety, and rollback
- Performance: CPU bottlenecks, memory pressure, queue growth, network overhead, database contention, resource utilization, and capacity limits
- Organizational boundaries: ownership clarity, cross-team dependencies, and process problems disguised as architecture problems
- Evolution cost: operational cost, team cognitive load, migration cost, and 1–3 year maintenance cost

## Usage Modes

| Mode | Use when | Output style |
|---|---|---|
| Quick scan | You want fast prioritization or a second opinion | 3–5 findings only |
| Focused review | You want one subsystem, service, module, migration, performance issue, or deployment path reviewed | Scoped findings and next steps |
| Full review | You want a broad system architecture assessment | Architecture model, findings, roadmap, uncertainty |
| Implementation mode | You want the agent to make small architecture-improving changes | Small edits, validation, summary |
| Design challenge mode | You want critique of a proposed design or migration plan | Assumptions, missing evidence, alternatives, cost comparison, recommendation, confidence |

## Non-goals

This skill does not:

- Recommend microservices by default
- Recommend rewrites without evidence
- Focus on code style
- Replace load testing or production validation
- Replace engineering judgment, incident review, or stakeholder context
- Assume the current architecture is wrong because it is not clean
- Hide uncertainty or invent missing operational context
- Treat formatting as an architecture issue unless it affects maintainability

## Templates

The skill includes reusable templates in `templates/`:

- `templates/checklists.md`
- `templates/architecture-review-report.md`
- `templates/improvement-roadmap.md`
- `templates/task-breakdown.md`
- `templates/adr-draft.md`

## Tested Models

This skill was prepared for use with:

- GPT-5 Codex
- Claude Sonnet
- Claude Opus

Gemini has not been verified in this repository yet. Add Gemini to this list after testing the skill with representative quick scan, focused review, design challenge, and implementation-mode prompts.
