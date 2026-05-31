---
name: research
description: Phase 2 of the product chain — research existing solutions, incumbents, demand signals, and market for a framed idea, fanning out parallel researchers.
argument-hint: "<slug>"
disable-model-invocation: true
---

# Research what exists (Phase 2)

Argument: `$ARGUMENTS` = the run `<slug>`.

1. Read `./products/<slug>/01-problem.md`, especially its `research-angles`. (If missing, run
   `/product-chain:frame <slug>` first.)
2. Spawn **several `market-researcher` subagents in parallel via the Agent tool — one per
   angle/incumbent.** Choose the count from how crowded the space looks (~2 niche, ~5+ crowded). Give
   each a focused brief (a specific competitor, segment, or demand question) and the run directory.
3. Synthesize their returned notes into `./products/<slug>/02-research.md`: incumbents & alternatives,
   demand signals, market size/trend, and the **differentiation gap** (where an opening exists, if
   any). Append a line to `LOG.md`.
4. Report the synthesis; next step is `/product-chain:validate <slug>`.
