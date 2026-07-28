---
name: code-simplify
description: Use when asked to simplify a Python script so that a beginner can read it line by line — without changing its behavior, outputs, or public interfaces.
---

# Code Simplify

## Overview

Rewrite a Python script for readability without touching its logic, outputs, or signatures. Four patterns apply universally. Always scan the repo first to know what you cannot break.

## Step 1 — Scan the repo for dependencies

Before changing anything:

1. Read all `src/*.py`, `experiments/*.py`, and `*.ipynb` files to find:
   - Any `import` or `from <script> import` statements referencing the target file
   - Function names called from outside the script
   - Parquet column names, file paths, and variable names that downstream scripts depend on
2. List what is off-limits: function signatures, output file paths, and written column names must not change.
3. Flag any callers that import specific names before proceeding — those signatures are frozen.

## Step 2 — Apply the four simplification patterns

### 1. Dense one-liners → named intermediate steps

Break any expression that chains more than two operations into named variables.

```python
# Before
var_th = float(np.mean(psi**2)) / (float(np.mean(T_resid**2))**2) / n

# After
mean_sq_influence = float(np.mean(influence_scores ** 2))
mean_sq_T_resid   = float(np.mean(T_resid ** 2))
var_theta = mean_sq_influence / (mean_sq_T_resid ** 2) / n
```

### 2. Terse variable names → full descriptive names

Rename everything: single-letter variables, abbreviations, and statistics symbols. No exceptions.

```python
# Before
m_t, tr_idx, psi, g, s, df, ns, wr, pq

# After
model_T, train_idx, influence_scores, group, swings, batter_table, swing_counts, whiff_rates, pitcher_quality
```

Do not rename parameters of functions called by other scripts — those are part of the public interface.

### 3. Nested / closure functions → module-level functions

Any function defined inside a loop or another function gets pulled out to module level with explicit parameters. Implicit closure over outer variables is what makes these hard to follow.

```python
# Before — defined inside a for loop, closes over `g` implicitly
def matched(feat, _g=g):
    ...
rows.append({"two_strike_rv_delta": matched("delta_run_exp")})

# After — module-level, explicit inputs
def _two_strike_penalty(group, feature):
    ...
rows.append({"two_strike_rv_delta": _two_strike_penalty(g, "delta_run_exp")})
```

### 4. Comprehension-heavy constructs → explicit operations

Replace dict/list comprehensions embedded in method calls with straightforward equivalent operations.

```python
# Before
grouped = u.groupby("batter_id").agg(
    n_swings=("n_swings", "sum"),
    **{c: (c, "sum") for c in agg_cols},
).reset_index()

# After — select columns first, then sum
grouped = weighted.groupby("batter_id")[["n_swings"] + agg_cols].sum().reset_index()
```

## Step 3 — Add short inline comments

Add one-line comments only where the WHY is non-obvious: a statistical formula step, a domain constraint, or a workaround a reader would need to look up. Do not comment what the code already says clearly.

```python
# Standardize to mean=0, SD=1 so theta is comparable across outcomes
T = (T_raw - T_raw.mean()) / T_raw.std()

# Weighted average: multiply by swing count, sum across stances, then divide
# This collapses switch hitters (two rows) into one batter-level row
for col in agg_cols:
    weighted[col] = weighted[col] * weighted["n_swings"]
```

## Step 4 — Verify nothing broke

1. Confirm output file names, written parquet column names, and function signatures are identical to the original.
2. If any public interface changed inadvertently, revert it.
3. Note any column renames or signature changes to the user before finishing — they may have downstream effects.
