---
name: code-explain
description: Use when the user asks you to explain what a piece of code does. Reads the full project scope first, then writes a per-function (or per-step) plain-English explanation suitable for a poster or non-technical reader, saved to a markdown file. Always runs /humanizer on the result before presenting it.
---

# Code Explain

## Overview

Explain code in a plain, poster-friendly voice. One section per function in file order. Each section leads with a bold **What it does** sentence, then a short bullet list in everyday English. No code variables, no math notation, no formulas. If the code uses a quantity like `sigma`, `config.horizon`, `belief_grid`, or `np.dot`, describe what it represents in words ("residual noise", "planning horizon", "the grid of possible confidence values", "a weighted sum") instead of naming the symbol. If the file has no functions, do the same structure step by step through the script in order.

The reader is a property manager, a non-technical collaborator, or a conference-poster viewer. They should be able to follow what the code does and why without any statistics or programming background.

## Step 1: Read the whole project first

Before writing anything, read `CLAUDE.md` (if present), every script in the project root, any READMEs, and run `ls` on the directory tree. The explanation must describe how the target code fits into the larger pipeline, not what the snippet does in isolation. Skipping this step produces explanations that miss upstream dependencies, downstream consumers, and documented design decisions.

## Step 2: Create the output file

Save the explanation next to the source file: same directory as the script, same basename, `.md` extension. For `foo/bar/script.py` the output is `foo/bar/script.md`. Overwrite without asking if the file already exists (the explanation is derived from the current source, so it should always reflect the latest code). Tell the user the path when you finish.

## Step 3: Write the explanation

Match this structure:

```
# <Script title in plain English>

`<path/to/script.py>`

<One short paragraph framing what the whole script does inside the pipeline, in plain English. Name the upstream inputs and downstream consumers by role, not by variable name.>

<Optional: a short bulleted list of the data holders or configuration objects the file defines before the first function, each described in one plain sentence.>

What follows is one plain-English section per function.

---

### `functionName`

**What it does.** <One sentence, no jargon.>

- <plain-English bullet about what it needs and where that comes from>
- <plain-English bullet about what it produces and who uses it>
- <plain-English bullets walking through the logic in order>

**Why <design choice>.** <Optional standalone paragraph for a non-obvious design decision worth defending. Only include when there is a real choice to explain.>

---

### `nextFunction`

...
```

### Section rules

- Heading: `` ### `functionName` ``. Do not include the argument list in the heading.
- First line of the body is always **What it does.** followed by one sentence.
- Bullets after that walk through the function in plain English. Describe inputs by their role ("the current confidence", "the account identifier"), not by parameter name. Describe outputs the same way.
- When the function performs a calculation, describe the calculation in words. "Shifts the baseline up or down by the Low or High receptivity gap" instead of `base + theta_i`. "Averages over plausible outcomes using a small set of representative points" instead of "Gauss-Hermite quadrature of order 9". A standard method's name can appear once in parentheses if it helps a technical reader, but the surrounding sentence must stand on its own without it.
- Any non-obvious design choice (why this algorithm, why this threshold, why this shape of state space) gets its own bold **Why ...** paragraph at the end of the section. Only include when there is a real choice to explain. Cite `CLAUDE.md` or project docs if the rationale is documented there.
- Separate sections with `---`.

### If the file has no functions

Walk through each logical step or cell in order. Heading format: `### Step N: <short label>`. Each step gets the same **What it does** sentence plus bullet list, same plain-English rules as above.

## Writing style

- Plain English throughout. If a sentence would confuse a reader who has never written code, rewrite it.
- No code variables, no code identifiers, no attribute paths. "The planning horizon" is fine; `config.horizon` is not.
- No formulas, no math notation, no Greek letters. Describe the math in words.
- No inline code quotes of the source. The explanation stands on its own; if a reader wants the code they can open the file.
- A standard algorithm or method can be named once in parentheses as a signpost for technical readers ("(backward induction)", "(Gauss-Hermite quadrature)"), but the surrounding sentence must still read naturally without that parenthetical.
- Specific over vague where it does not require jargon. "Posts every day no matter the confidence" beats "Always posts".
- No marketing language, no "serves as", no rule-of-three padding, no trailing summaries.
- No em or en dashes anywhere. ASCII hyphens only.

## Step 4: Run /humanizer

After the file is written, invoke `/humanizer` on it. Only present the result to the user after /humanizer has run.

## Quality checks

- Every function (or step) in the source file has its own section.
- Every section leads with a bold **What it does.** sentence before the bullets.
- No code variables, no attribute paths, no formulas, no Greek letters anywhere in the output.
- Design decisions worth defending have their own **Why ...** paragraph.
- Design decisions documented in `CLAUDE.md` or READMEs are cited, not paraphrased from scratch.
- No em/en dashes anywhere.
- `/humanizer` has been run.
