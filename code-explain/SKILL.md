---
name: code-explain
description: Use when the user asks you to explain what a piece of code does — requires reading the full project scope first, then producing a two-layer explanation (technical + domain layman's terms) in bullet-point form, grounded in whole-project context.
---

# Code Explain

## Overview

Explain code in two layers: technical (for someone who can read but not deeply write code) and plain-language domain analogy (e.g., baseball for a sports analytics project). Every bullet must be grounded in the full project context — not the snippet in isolation.

## Step 1 — Read the Entire Project First (Required)

Before writing a single word of explanation, read **all** of the following:

- `CLAUDE.md` (if present) — pipeline architecture, constants, design decisions, key terminology
- Every script in the project root (`.R`, `.py`, `.sql`, `.sh`, etc.) — at minimum skim for structure and how they interrelate
- Any `README` files
- Directory structure (`ls` or equivalent) to understand what files exist

**Only proceed to explanation after you have a complete picture of how the target code fits into the larger system.** If you explain before reading the project, you will miss upstream dependencies, downstream consumers, and documented design rationale.

## Step 2 — Technical Explanation

Write bullet points covering, in order:

- **Role in pipeline/system** — what stage or purpose this code serves relative to the whole project
- **Inputs** — what data or arguments come in and where they originate in the project
- **Core logic** — how data is transformed, filtered, modeled, or routed; call out non-obvious thresholds, priors, windows, fixed effects, or algorithmic choices
- **Outputs** — what this produces and what downstream code or users consume it
- **Key dependencies** — which other scripts, checkpoints, packages, or constants this relies on
- **Design decisions** — if CLAUDE.md or a README documents WHY something is done a certain way, cite it explicitly (e.g., "pitch count excluded per BIC criterion — see CLAUDE.md")

One idea per bullet. No introductory prose. No summary paragraph at the end.

## Step 3 — Layman's Explanation

Re-explain the same code using domain analogies in the same bullet order so the reader can cross-reference:

- **Baseball / sports analytics projects**: frame logic as scouting decisions, in-game strategy, box-score math, or on-field action. Use terms the user already knows: ERA, xwOBA, pitch count, pull the starter, etc.
- **Other domains**: identify the domain from CLAUDE.md or project context and match analogies accordingly.

Each technical bullet maps to exactly one plain-language counterpart. Keep the mapping explicit — don't introduce new concepts in the layman's section.

## Output Format

```
## Technical Explanation
- [bullet]
- [bullet]
...

## In Plain Terms ([Domain])
- [bullet matching technical bullet 1]
- [bullet matching technical bullet 2]
...
```

No prose paragraphs. No headers within bullet sections. No trailing summary.

## Quality Checks

Before responding, verify:

- Every technical bullet references how the code connects to the rest of the project (not just what the function does in isolation)
- Layman's bullets use domain vocabulary naturally — no forced analogies
- Any design decision documented in CLAUDE.md is cited, not paraphrased from scratch
- If code behavior contradicts or is absent from project documentation, flag it explicitly as a discrepancy
