---
name: prd
description: The format for a product-chain blueprint (04-blueprint.md) — feasibility, PRD, stack choice, workstreams, and decisions for a human.
---

# Blueprint format → `04-blueprint.md`

Turn a validated problem into something buildable. Scope to the smallest thing that tests the riskiest
assumption (MVP), not the full vision. Method cues: Working Backwards, RICE for ordering.

```markdown
# Blueprint: <slug>

## Feasibility
Can this be built, by whom, in what rough time, with what hard dependencies/risks? Flag anything that
needs the user (accounts, APIs, budget, legal/ToS — e.g. scraping a site that forbids it).

## PRD
- **Problem recap** — one line from `01-problem.md`.
- **Users** — who the MVP serves first.
- **MVP scope** — the few capabilities that test the riskiest assumption. Order by RICE.
- **Out of scope (v1)** — what we deliberately defer.
- **Success metrics** — how we'll know the MVP worked.

## Stack choice
The technology, with a one-line rationale per choice. Prefer the simplest stack that ships; respect
any constraint stated in the idea.

## Workstreams
Ordered, buildable chunks (independent ones marked parallel-safe). Each: name, what it delivers,
depends-on. These map 1:1 to builder subagents.

## decisions-for-human
Explicit choices the build should NOT make alone: pricing, third-party accounts/keys, data sources
with legal/ToS implications, hosting, naming, anything irreversible. Each with options + a default.
```

If feasibility is blocked on something only the user can provide, say so before listing workstreams.
