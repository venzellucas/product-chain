---
name: build
description: Phase 5 of the product chain — implement the solution from an approved blueprint, fanning out builders across workstreams.
argument-hint: "<slug>"
disable-model-invocation: true
---

# Build the solution (Phase 5)

Argument: `$ARGUMENTS` = the run `<slug>`.

1. Read `./products/<slug>/04-blueprint.md`. If it doesn't exist, run `/product-chain:blueprint
   <slug>` first. Confirm the user has approved the plan (Gate G3) before writing any code.
2. Read the `workstreams` and the `decisions-for-human`. If any flagged decision is still unanswered,
   ask the user now.
3. Spawn **`builder` subagents via the Agent tool**, one per workstream — run independent workstreams
   in parallel, dependent ones in order. Each builder works inside `./products/<slug>/build/` and
   returns what it produced. (Builders can't spawn sub-builders, so you sequence them here.)
4. After building, verify it runs (build/test/launch as appropriate), append a line to `LOG.md`, and
   report what was built, how to run it, and any remaining flagged decisions.
