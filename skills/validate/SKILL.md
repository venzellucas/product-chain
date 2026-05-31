---
name: validate
description: Phase 3 of the product chain — judge whether the pain is real and worth building, producing a go/pivot/kill verdict with confidence and risks.
argument-hint: "<slug>"
disable-model-invocation: true
---

# Validate the pain (Phase 3)

Argument: `$ARGUMENTS` = the run `<slug>`.

1. Read `01-problem.md` and `02-research.md` from `./products/<slug>/`. (If research is missing, run
   `/product-chain:research <slug>` first.)
2. Delegate to the **`pain-validator`** subagent (via the Agent tool), passing the run directory. It
   writes `./products/<slug>/03-validation.md` with a **GO / PIVOT / KILL** verdict, confidence, a
   steelman case, a skeptic case, the risks both agree on, and what would change the verdict.
3. Append a line to `LOG.md`.
4. **Present the verdict to the user** and ask with **AskUserQuestion**: Continue to blueprint /
   Pivot / Stop. On Pivot, capture the adjustment, update `01-problem.md`, and point back to
   `/product-chain:research <slug>`. On Continue, point to `/product-chain:blueprint <slug>`.
