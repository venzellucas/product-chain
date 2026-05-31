---
name: product
description: Run the full product chain on a raw idea — frame the problem, research what exists, validate go/pivot/kill, blueprint, and build, pausing only at the human decisions. Use when someone hands you a product idea or pain to take from concept to solution.
argument-hint: "<one-line idea or pain>"
---

# Product chain — orchestrator

You are driving an **idea → validated → built** pipeline. Run it mostly autonomously; stop only at the
gates below. The idea is:

> $ARGUMENTS

Today (UTC): !`date -u +%Y-%m-%dT%H:%M:%SZ`

## Setup

1. If `$ARGUMENTS` is empty, ask the user for the idea, then continue.
2. Derive a short kebab-case `<slug>` from the idea (e.g. `freelancer-invoice-reminders`).
3. **Resume check:** if `./products/<slug>/` already exists, read its files + `LOG.md` and continue
   from the first phase whose output is missing. Otherwise create `./products/<slug>/` and write
   `00-idea.md` with the verbatim idea and the timestamp above.
4. All artifacts go in `./products/<slug>/` in the **current working directory** — never inside the
   plugin. Append a one-line entry to `LOG.md` after every phase and every gate.

## Delegate to subagents

Use the **Agent tool** to run each phase's worker (subagent types: `problem-framer`,
`market-researcher`, `pain-validator`, `solution-architect`, `builder` — namespaced
`product-chain:<name>` when installed as a plugin). Pass each subagent the run directory path and tell
it which doc to read and which to write. Subagents cannot ask the user or spawn other subagents — **you**
own all fan-out and all gates.

## Run the phases

**Phase 1 — Frame.** Delegate to `problem-framer` → `01-problem.md`.
→ **Gate G1 (soft):** continue automatically. Only if the idea is too ambiguous to research (no clear
user or problem), ask the user **one** clarifying question, then continue.

**Phase 2 — Research.** Read the `research-angles` in `01-problem.md`. Spawn **several
`market-researcher` subagents in parallel — one per angle/incumbent** (decide the count from how
crowded the space looks: ~2 for a niche idea, ~5+ for a crowded one). Each returns notes; you
synthesize them into `02-research.md`.

**Phase 3 — Validate.** Delegate to `pain-validator` → `03-validation.md` (a GO / PIVOT / KILL verdict
with confidence, a steelman case, a skeptic case, the risks both agree on, and what would change the
verdict).
→ **Gate G2 (HARD):** present the verdict + key evidence and ask the user with **AskUserQuestion**:
**Continue to blueprint** / **Pivot** (capture the adjustment, update `01-problem.md`, loop back to
Phase 2) / **Stop**. Never blueprint on a KILL without an explicit override.

**Phase 4 — Blueprint.** Delegate to `solution-architect` → `04-blueprint.md` (feasibility + PRD +
stack choice + workstreams + a `decisions-for-human` list).
→ **Gate G3 (HARD):** present the plan summary and the flagged decisions; get explicit approval (and
answers to any flagged decisions) before any build.

**Phase 5 — Build.** Read the workstreams in `04-blueprint.md`. Spawn `builder` subagents (parallel
where workstreams are independent) writing into `./products/<slug>/build/`. Surface only the
architect-flagged decisions; otherwise build autonomously. Report what was built and how to run it.

## Finish

Summarize: the verdict, what was built, where it lives, and the open `decisions-for-human`. Keep the
running `LOG.md` current so the chain can be resumed or re-run per phase later.
