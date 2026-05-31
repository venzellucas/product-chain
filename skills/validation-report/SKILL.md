---
name: validation-report
description: The format for a product-chain validation verdict (03-validation.md) — go/pivot/kill with confidence, steelman, skeptic, and agreed risks.
---

# Validation format → `03-validation.md`

Judge whether the pain is real and worth building, grounded only in `01-problem.md` and
`02-research.md` (don't invent evidence). Run a **dual pass**: argue hard *for*, then argue hard
*against*, and trust the risks both passes surface — they're the real ones.

```markdown
# Validation: <slug>

## Verdict: GO | PIVOT | KILL
**Confidence:** low | medium | high — and the one-line reason.

## Steelman (the strongest honest case FOR)
Why this is worth building: the pain, the demand, the gap, the wedge.

## Skeptic (the strongest honest case AGAINST)
Why this fails: weak demand, crowded space, no reachable user, no willingness to pay, thin moat.

## Agreed risks (surfaced by both sides)
The 2–4 risks that survive both lenses. These are what to de-risk first.

## What would change this verdict
The cheapest evidence that would flip GO↔KILL (e.g. "10 target users confirm they'd pay $X").

## Recommendation
GO → proceed to blueprint. PIVOT → the specific adjustment to re-frame and re-research. KILL → why,
and what to learn from it.
```

PIVOT means the *problem or user* should change, not "try harder." KILL is a legitimate, valuable
outcome — say so plainly.
