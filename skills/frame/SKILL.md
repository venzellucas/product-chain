---
name: frame
description: Phase 1 of the product chain — turn a raw idea into a sharp problem statement, target user, riskiest assumptions, success metric, and research angles.
argument-hint: "<slug | idea>"
disable-model-invocation: true
---

# Frame the problem (Phase 1)

Argument: `$ARGUMENTS` (a `<slug>` of an existing run, or a raw idea to start a new one).

1. If it's a new idea, derive a kebab-case `<slug>`, create `./products/<slug>/`, and write
   `00-idea.md` (verbatim idea + timestamp). If it's an existing slug, read `00-idea.md`.
2. Delegate to the **`problem-framer`** subagent (via the Agent tool), passing the run directory.
3. Ensure it writes `./products/<slug>/01-problem.md` and append a line to `LOG.md`.
4. Report the problem statement and the research angles, and remind the user the next step is
   `/product-chain:research <slug>` (or `/product-chain:product` to run the whole chain).
