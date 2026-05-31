# product-chain

An autonomous **product chain** for Claude Code. Hand it a raw idea or pain, and it drives the whole
arc — frame the problem, research what already exists, judge **go / pivot / kill**, blueprint, and
build — running on its own and pausing only at the few decisions a human should own.

It's the connective tissue that existing tools skip: idea-validators don't build, and AI builders
don't validate. This chains both, with gates in between.

## Install

```text
/plugin marketplace add venzellucas/product-chain
/plugin install product-chain@venzel-tools
```

Then, from any project directory:

```text
/product-chain:product "a tool that reminds freelancers to invoice on time"
```

No setup, no API keys — it uses Claude Code's built-in web search. (You'll approve a WebSearch
permission once; web search is US-region.)

## How it works

| Phase | What happens | Output | Human gate? |
|------|--------------|--------|-------------|
| 1. Frame | Sharp problem statement, target user, riskiest assumptions, research angles | `01-problem.md` | soft |
| 2. Research | Parallel researchers sweep incumbents, demand, market, and the gap | `02-research.md` | — |
| 3. Validate | Go/pivot/kill verdict with confidence + the risks a steelman and a skeptic both agree on | `03-validation.md` | **yes** |
| 4. Blueprint | Feasibility, PRD, stack choice, build workstreams, decisions-for-human | `04-blueprint.md` | **yes** |
| 5. Build | Builders implement the workstreams | `build/` | flagged decisions only |

Everything for a run lands in **your** working directory:

```
./products/<slug>/
  00-idea.md  01-problem.md  02-research.md  03-validation.md  04-blueprint.md  build/  LOG.md
```

Each phase reads the previous document and writes the next, so every run is auditable and resumable.

### Run the whole chain, or one phase at a time

- `/product-chain:product "<idea>"` — the orchestrator; runs all five phases with the gates.
- `/product-chain:frame <slug|idea>` · `:research <slug>` · `:validate <slug>` · `:blueprint <slug>`
  · `:build <slug>` — run or re-run a single phase (resume a run, or redo one step).

### Autonomy

Runs unattended through framing and research. It stops to ask you only twice — **"is this worth
building?"** (after validation) and **"approve the plan?"** (after the blueprint) — plus any decision
the architect explicitly flagged during build (pricing, accounts/keys, hosting, anything
irreversible). Tune this in `CLAUDE.md`.

## Publishing your own copy (for maintainers)

This repo is **both the plugin and its marketplace** (`.claude-plugin/plugin.json` +
`.claude-plugin/marketplace.json`). To share it:

1. `claude plugin validate` — check the manifests and structure.
2. Test locally without installing: `claude --plugin-dir ./product-chain`, then run
   `/product-chain:product "..."` from a scratch directory.
3. `git init && git add -A && git commit -m "product-chain" && git push` to a GitHub repo (a
   **private** repo works fine for an internal/team audience — colleagues just need read access).
4. Colleagues run the two install commands above. Ship updates by bumping `version` in
   `plugin.json`, pushing, and having users run `/plugin marketplace update`.

Three identifiers, easy to confuse:

| Identifier | Defined in | Used as |
|---|---|---|
| repo path | GitHub | what you `/plugin marketplace add` (`venzellucas/product-chain`) |
| marketplace name | `marketplace.json → name` | the part after `@` (`venzel-tools`) |
| plugin name | `plugin.json → name` | the part before `@` (`product-chain`) |

→ `/plugin install product-chain@venzel-tools`.

For a whole team, auto-register the marketplace via `extraKnownMarketplaces` in managed settings so
nobody runs `marketplace add`. To reach strangers, submit to Anthropic's community marketplace at
[claude.ai/settings/plugins/submit](https://claude.ai/settings/plugins/submit).

## License

MIT.
