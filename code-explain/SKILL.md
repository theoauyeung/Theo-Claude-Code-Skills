---
name: code-explain
description: Use when the user asks you to explain what a piece of code does. Requires reading the full project scope first, then producing a concise two-layer explanation (technical + domain layman's terms) grounded in whole-project context.
---

# Code Explain

## Step 1: Read the entire project first (required)

Before writing anything, read `CLAUDE.md` (if present), every script in the project root, any READMEs, and run `ls` to understand the directory structure. Only proceed after you have a complete picture of how the target code fits into the larger system. Explaining before reading the project produces explanations that miss upstream dependencies, downstream consumers, and documented design decisions.

## Step 2: Technical explanation

Write one or two concise paragraphs covering: what role this code plays in the pipeline, what inputs it takes and where they come from, what the core logic does (call out non-obvious thresholds, priors, windows, or algorithmic choices explicitly), what it outputs and what consumes those outputs, and any design decisions documented in CLAUDE.md or a README (cite them directly, don't paraphrase).

Keep it tight. One idea per sentence. If CLAUDE.md explains *why* something is done a certain way, say so. Don't rediscover it.

## Step 3: Layman's explanation

Re-explain the same code in one or two paragraphs using domain analogies, in the same order as the technical explanation so the reader can cross-reference. For baseball/sports analytics projects, frame logic as scouting decisions, in-game strategy, box-score math, or on-field action. Use terms the reader already knows: ERA, xwOBA, pitch count, pull the starter. Each concept in the technical explanation maps to exactly one plain-language counterpart. Don't introduce new ideas here.

## Writing style (apply throughout)

Apply humanizer principles to both explanations:

- Use "is/are/has" rather than "serves as", "stands as", or "boasts"
- Cut AI vocabulary: additionally, crucial, pivotal, highlight, underscore, showcase, tapestry, landscape, testament, vibrant
- No -ing phrase tacking onto sentence ends ("...ensuring that", "...reflecting the", "...showcasing how")
- No rule of three. If two examples cover it, stop at two
- Vary sentence length. Short sentences after long ones
- Be specific. "drops 6 inches after commit" beats "generates significant late movement"
- No vague attributions ("experts believe", "research suggests"). Cite the actual source or drop the claim.

## Output format

```
## Technical explanation
[one or two paragraphs]

## In plain terms ([domain])
[one or two paragraphs]
```

No headers within paragraphs. No trailing summary. No "I hope this helps."

## Quality checks

Before responding: confirm every sentence connects the code to the rest of the project, not just what the function does in isolation. If code behavior contradicts or is absent from project documentation, flag it explicitly.
