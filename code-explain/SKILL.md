---
name: code-explain
description: Use when the user asks you to explain what a piece of code does. Requires reading the full project scope first, then producing a per-function (or per-step) explanation that covers both overall purpose and logic intricacies, written in concise bullet points with no jargon, saved as a markdown file. Always run /humanizer before presenting the result.
---

# Code Explain

## Step 1: Read the entire project first (required)

Before writing anything, read `CLAUDE.md` (if present), every script in the project root, any READMEs, and run `ls` to understand the directory structure. Only proceed after you have a complete picture of how the target code fits into the larger system.

## Step 2: Create the output markdown file

Create a file at `docs/explanations/<script-name>.md` (create `docs/explanations/` if it doesn't exist). Write the explanation directly into this file. Tell the user where you saved it.

## Step 3: Write the explanation

Mirror the approach in `Shared Work/features/feature_engineering.md` exactly.

**If the code has functions:**

For each function, write:
1. A one-line summary of what the function does overall
2. A bullet list covering:
   - What the function takes in and where those inputs come from
   - What it produces and what consumes it downstream
   - How the logic works step by step — name the actual operations, column names, thresholds, and formulas rather than describing them vaguely
   - Any non-obvious design choice: why a particular algorithm, threshold, or formula was chosen
   - Any leakage guard, fallback, or edge case handled

Use `### \`functionName(args)\`` as the heading for each function section.

**If the code has no functions (e.g., a notebook or a flat script):**

Walk through each logical step or cell in order. For each step:
1. A one-line summary of what that step does
2. A bullet list with the same content as above (inputs, outputs, logic, design choices, edge cases)

Use `### Step N: <short label>` as the heading for each step.

## Writing style (apply throughout)

- No jargon — write as if explaining to someone who has never seen the codebase
- Bullet points only — no paragraphs, no full prose sections
- Be specific: name the actual column, threshold, or function rather than describing it vaguely
- Use "is/are/has" rather than "serves as" or "stands as"
- Cut AI vocabulary: additionally, crucial, pivotal, highlight, underscore, showcase, tapestry, vibrant
- No -ing phrases tacked onto sentence ends ("...ensuring that", "...reflecting the")
- No vague attributions ("research suggests"). Cite the actual source or drop the claim.
- No trailing summary. No "I hope this helps."

## Output format

```markdown
# Explanation: <script name>

`<relative path to the file>`

Each section below gives a one-line summary of what the function (or step) does, followed by bullets that explain specifically how the code builds it.

---

### `functionName(args)`

One-line summary of purpose.

- <bullet about inputs / outputs>
- <bullet about a specific logic step>
- <bullet about a design choice or threshold>
- <bullet about a leakage guard or edge case>

---

### `nextFunction(args)`

...
```

## Quality checks

Before saving: confirm every bullet connects the code to the rest of the project. If code behavior contradicts project documentation, flag it with a `> ⚠️ Note:` blockquote in the file.

## Final step: run /humanizer

After writing and saving the file, invoke `/humanizer` on the full document before presenting anything to the user. Do not show the explanation until /humanizer has run.
