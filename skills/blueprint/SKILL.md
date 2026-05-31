---
name: blueprint
description: Phase 4 of the product chain — turn a validated problem into a feasibility analysis, PRD, stack choice, and build workstreams.
argument-hint: "<slug>"
disable-model-invocation: true
---

# Blueprint the solution (Phase 4)

Argument: `$ARGUMENTS` = the run `<slug>`.

1. Read `01-problem.md`, `02-research.md`, and `03-validation.md` from `./products/<slug>/`. If the
   verdict is **KILL**, stop and tell the user (don't blueprint without an explicit override).
2. Delegate to the **`solution-architect`** subagent (via the Agent tool), passing the run directory.
   It writes `./products/<slug>/04-blueprint.md`: feasibility, a PRD (problem recap, users, MVP scope,
   out-of-scope, success metrics), a **stack choice with rationale**, ordered **workstreams**, and a
   **`decisions-for-human`** list.
3. Append a line to `LOG.md`.
4. **Present the plan summary + flagged decisions** and get explicit approval (and answers to the
   flagged decisions) before any build. Next step: `/product-chain:build <slug>`.
