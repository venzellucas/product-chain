# product-chain — operating manual

An autonomous **product chain**: takes a raw idea and drives it through five phases, pausing only at
the decisions a human must own. This file is the canonical spec for the pipeline, the artifact
contract, the gates, and the autonomy policy. (It loads when you develop *inside* this repo. At
runtime in a user's session, the orchestrator skill restates what it needs — a plugin's CLAUDE.md is
not loaded into installed users' sessions.)

## The pipeline

| # | Phase | Skill | Subagent | Output |
|---|-------|-------|----------|--------|
| 1 | Frame | `/product-chain:frame` | `problem-framer` | `01-problem.md` |
| 2 | Research | `/product-chain:research` | `market-researcher` (×N parallel) | `02-research.md` |
| 3 | Validate | `/product-chain:validate` | `pain-validator` | `03-validation.md` |
| 4 | Blueprint | `/product-chain:blueprint` | `solution-architect` | `04-blueprint.md` |
| 5 | Build | `/product-chain:build` | `builder` (×N parallel) | `build/` |

`/product-chain:product "<idea>"` is the **orchestrator** — it runs all five end-to-end, enforcing the
gates. Each phase skill can also run standalone against an existing run to resume or re-run a phase.

## Artifact contract (the backbone)

Every run lives in **the user's working directory**, never inside the plugin:

```
./products/<slug>/
  00-idea.md         # raw input, verbatim, + ISO timestamp
  01-problem.md      # problem statement, target user, assumptions (riskiest first), success metric,
                     #   research-angles (the incumbents/queries phase 2 should chase)
  02-research.md     # incumbents & alternatives, demand signals, market size/trend, differentiation
                     #   gap, synthesis
  03-validation.md   # VERDICT: GO | PIVOT | KILL · confidence · steelman · skeptic · agreed risks ·
                     #   "what would change this verdict"
  04-blueprint.md    # feasibility, PRD, stack choice + rationale, workstreams, decisions-for-human
  build/             # the actual implementation
  LOG.md             # append-only: ISO timestamp · phase · what happened · gate outcome
```

`<slug>` is short kebab-case derived from the idea (e.g. `freelancer-invoice-reminders`). Each phase
**reads the previous numbered doc and writes the next** — structured handoffs, not free chat. This is
what makes it a chain and keeps every run auditable.

## Gates (where a human is pulled in)

- **G1 — after Frame (soft):** continue automatically. Pause only if the idea is too ambiguous to
  research responsibly (unknown user, unknown problem). Then ask one clarifying question.
- **G2 — after Validate (HARD):** present the verdict and ask: **Continue → blueprint / Pivot
  (re-frame with an adjustment) / Stop.** Never proceed to blueprint on a KILL without explicit
  override.
- **G3 — after Blueprint (HARD):** present the plan summary and the flagged decisions; get explicit
  approval before **any** build work.
- **During Build:** surface only the decisions the architect flagged in `04-blueprint.md`
  (`decisions-for-human`). Everything else runs autonomously.

Append every gate outcome to `LOG.md`.

## Autonomy policy (default — change here)

Runs **unattended through Frame + Research**. Stops at **G2** ("is it worth building?") and **G3**
("approve the plan") — the two decisions that are genuinely the human's. During build, asks only
about architect-flagged forks. To make it more autonomous, soften G3 or pre-answer flagged decisions
in the idea. To make it more cautious, promote G1 to hard.

## Platform notes (why the design is shaped this way)

- **Subagents can't spawn subagents and can't call AskUserQuestion.** So the orchestrator (running in
  the main session) owns all fan-out and all gates; subagents are pure workers that read/write the
  artifact docs and return.
- **Installed plugins are read-only** (copied to `~/.claude/plugins/cache`). Always write run
  artifacts to the user's cwd (`./products/...`); read bundled assets via `${CLAUDE_PLUGIN_ROOT}`.
- **Fan-out** uses the standard subagent (Agent/Task) tool, available on every plan. On Max/Team,
  dynamic workflows can run the research/build fan-out faster, but nothing depends on it.

## Conventions

- Subagents are namespaced when installed: `product-chain:problem-framer`, etc. Delegate by that type.
- Keep skill bodies concise — they persist in context across turns.
- Deliverable formats live in the template skills (`problem-statement`, `competitive-analysis`,
  `validation-report`, `prd`); subagent prompts embed the same structure so they're self-contained.
