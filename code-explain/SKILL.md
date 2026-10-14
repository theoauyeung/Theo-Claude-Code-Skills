---
name: code-explain
description: Use when the user asks you to explain what a piece of code does. Reads the full project scope first, then writes a per-function (or per-step) bullet-point explanation grounded in whole-project context, saved to a markdown file. Always runs /humanizer on the result before presenting it.
---

# Code Explain

## Overview

Explain code the exact way `Shared Work/features/feature_engineering.md` explains `feature_engineering.py`: one section per function in file order, each led by a one-line summary and followed by bullets that spell out specifically how that function works. No "technical vs. plain terms" split. No analogies. No overview paragraphs beyond a short file header. If the file has no functions, do the same structure step by step through the script in order.

## Step 1: Read the whole project first

Before writing anything, read `CLAUDE.md` (if present), every script in the project root, any READMEs, and run `ls` on the directory tree. The explanation must describe how the target code fits into the larger pipeline, not what the snippet does in isolation. Skipping this step produces explanations that miss upstream dependencies, downstream consumers, and documented design decisions.

## Step 2: Create the output file

Save the explanation next to the source file: same directory as the script, same basename, `.md` extension. For `foo/bar/script.py` the output is `foo/bar/script.md`. Overwrite without asking if the file already exists (the explanation is derived from the current source, so it should always reflect the latest code). Tell the user the path when you finish.

## Step 3: Write the explanation

Match the structure of `feature_engineering.md` exactly:

```
# Explanation: <script-name>.py

`<path/to/script.py>`

<one or two sentences framing what the whole script does inside the pipeline>

---

### `functionName(args)`

<One-line summary of what the function does.>

- <specific bullet about a piece of logic>
- <next bullet>
- ...

---

### `nextFunction(args)`

...
```

### If the file has functions

One section per function, in the order they appear in the source. Heading format: `` ### `functionName(args)` ``. Each section is:

1. A one-line summary of what the function does overall.
2. A bullet list covering:
   - What the function takes in and where those inputs originate upstream.
   - What it produces and what consumes it downstream.
   - How the logic works step by step. Name real column names, constants, thresholds, and formulas; quote the actual code when it clarifies the point (e.g. `` `df["caption"].str.len().fillna(0).astype(int)` `` and then say what it does).
   - Any non-obvious design choice: why this algorithm, threshold, or formula was chosen. Cite `CLAUDE.md` or project docs if the rationale is documented there.
   - Any leakage guard, fallback, or edge case handled.

Separate sections with `---`.

### If the file has no functions

Walk through each logical step or cell in order. Heading format: `### Step N: <short label>`. Each step gets the same one-line summary plus bullet list as above.

## Writing style

- Specific over vague. "Rows below 0.50 are flagged `pillar_needs_review = True`" beats "Low-confidence rows are flagged."
- Name real identifiers (columns, constants, filenames). No abstractions where the real name fits.
- Quote the actual code inline when it clarifies the explanation.
- If the code mirrors a cited paper or method, cite it the way the source file cites it.
- No marketing language, no "serves as," no rule-of-three padding, no trailing summaries.
- No em or en dashes anywhere. ASCII hyphens only.

## Step 4: Run /humanizer

After the file is written, invoke `/humanizer` on it. Only present the result to the user after /humanizer has run.

## Quality checks

- Every function (or step) in the source file has its own section.
- Every section leads with a one-line summary before the bullets.
- Bullets name real identifiers rather than abstractions.
- Design decisions documented in `CLAUDE.md` or READMEs are cited, not paraphrased from scratch.
- No em/en dashes anywhere.
- `/humanizer` has been run.
