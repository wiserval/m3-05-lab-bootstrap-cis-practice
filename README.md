![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Lab | Bootstrap CIs in Practice

## Overview

Confidence intervals let you move beyond a single-number estimate and say, "I'm reasonably sure the true value falls somewhere in this range." In the lesson you learned how the bootstrap — resampling with replacement from your own data — gives you a flexible, assumption-light way to build those intervals.

In this lab you will put that idea into practice. You will write a reusable `bootstrap_ci` function, apply it to real-world data to estimate means, medians, and proportions, and then compare your bootstrap intervals with the classical normal-approximation CIs from `scipy`. By the end you will also produce clean visualisations of the resampling distributions and write a short interpretation paragraph — the kind of summary a stakeholder or manager could actually read and act on.

This lab bridges the gap between understanding the bootstrap concept and using it confidently in an analysis workflow. The skills you build here will serve you whenever a formula-based interval is unavailable or its assumptions feel shaky.

## Learning Goals

By the end of this lab, you should be able to:

- Implement a general-purpose bootstrap confidence interval function in Python
- Apply bootstrap resampling to estimate CIs for different statistics (mean, median, proportion)
- Use `scipy.stats` to compute normal-approximation confidence intervals
- Compare bootstrap and parametric intervals and explain when they diverge
- Visualise a bootstrap sampling distribution with clear, labelled plots
- Write a concise, non-technical interpretation of confidence interval results

## Setup and Context

You will work in a Jupyter notebook throughout this lab. The dataset you will use contains realistic numerical and categorical variables so you can practise estimating different kinds of statistics. All analysis, code, and written answers belong in the same notebook.

### Prerequisites

- Familiarity with Python functions, loops, and NumPy arrays
- Understanding of sampling distributions and the bootstrap concept (covered in the lesson)
- Basic matplotlib / seaborn plotting skills

## Requirements

1. **Fork** this repo to your GitHub account, then **clone** your fork locally.
2. Make sure you have the following Python packages installed:

```bash
pip install numpy pandas scipy matplotlib seaborn
```

3. Work inside the notebook specified in **Getting Started**.

## Getting Started

1. Create a new Jupyter notebook called **`m3-05-bootstrap-cis-practice.ipynb`** in the root of your cloned repo.
2. At the top of the notebook, import the libraries you will need:

```python
import numpy as np
import pandas as pd
import scipy.stats as st
import matplotlib.pyplot as plt
import seaborn as sns

np.random.seed(42)
```

3. Work through the tasks below in order. Each task should have its own clearly labelled section (Markdown heading) in the notebook.

## Tasks

### Task 1: Build a Reusable `bootstrap_ci` Function

Write a function with the following signature:

```python
def bootstrap_ci(data, stat_func=np.mean, n_boot=10_000, ci_level=0.95):
    """
    Returns (lower, upper) percentile bootstrap CI.

    Parameters
    ----------
    data : array-like
        The sample to resample from.
    stat_func : callable
        The statistic to compute on each resample (default: np.mean).
    n_boot : int
        Number of bootstrap resamples.
    ci_level : float
        Confidence level (e.g. 0.95 for a 95 % CI).
    """
```

**Steps:**

1. Inside the function, use `np.random.choice` (with replacement) to draw `n_boot` resamples of the same size as `data`.
2. Compute `stat_func` on each resample and store the results in an array called `boot_stats`.
3. Use `np.percentile` to extract the lower and upper bounds based on `ci_level`. For a 95 % CI these are the 2.5th and 97.5th percentiles.
4. Return `(lower, upper)`.

**Validation:** Call your function on `np.arange(1, 101)` with the default mean. The 95 % CI should be roughly (40, 60). Verify this before moving on.

### Task 2: Apply Bootstrap CIs to Real Data

Load or generate a dataset with at least 200 rows that includes:

- A **continuous numeric** column (e.g. income, test score, response time)
- A **binary / boolean** column (e.g. purchased yes/no, passed yes/no)

You may use any publicly available dataset or generate synthetic data with `np.random.normal` / `np.random.binomial`.

**Compute the following bootstrap CIs (95 %, 10 000 resamples each):**

| # | Statistic | Column | `stat_func` |
|---|-----------|--------|-------------|
| 1 | Mean | Continuous column | `np.mean` |
| 2 | Median | Continuous column | `np.median` |
| 3 | Proportion | Binary column | `np.mean` (on 0/1 values) |

Present the three intervals in a tidy summary table using `pd.DataFrame`.

### Task 3: Compare Bootstrap CIs with Normal-Approximation CIs

For the **mean** and the **proportion** from Task 2, compute classical normal-approximation 95 % CIs:

- **Mean CI:** Use `st.t.interval` (or `st.norm.interval`) with the sample mean and standard error.
- **Proportion CI:** Use the Wald interval formula: p̂ ± z × √(p̂(1 − p̂) / n), where z = 1.96.

Create a comparison table with columns: `Statistic`, `Bootstrap Lower`, `Bootstrap Upper`, `Normal Lower`, `Normal Upper`.

**Answer in a Markdown cell:**

1. Are the two approaches giving similar intervals? Where do they diverge, if at all?
2. For which statistic (mean vs. proportion vs. median) is the bootstrap approach especially useful, and why?

### Task 4: Visualise the Bootstrap Distribution

For **one** of the statistics from Task 2, create a polished plot that includes:

1. A histogram (or KDE) of the 10 000 bootstrap estimates.
2. Vertical dashed lines marking the lower and upper bounds of the 95 % CI.
3. A vertical solid line marking the observed sample statistic.
4. Clear axis labels, a title, and a legend.

**Stretch goal:** Create a 1 × 3 subplot figure showing the bootstrap distributions for all three statistics side by side.

Use `matplotlib` or `seaborn`. Make sure the figure is presentation-ready (appropriate font sizes, no overlapping labels).

### Task 5: Write an Interpretation Paragraph

In a Markdown cell, write a **3–5 sentence paragraph** that a non-technical stakeholder could understand. Your paragraph should:

- State the point estimate and its 95 % CI.
- Explain what the interval means in plain language (avoid "there is a 95 % probability that…" — use the frequentist interpretation).
- Note whether the interval is wide or narrow and what that implies about the precision of the estimate.
- Suggest one action or next step based on the result (e.g. "collect more data", "the effect is large enough to act on").

**Tip:** Read your paragraph out loud. If it sounds like a stats textbook, rewrite it.

## Submission

### What to submit

- `m3-05-bootstrap-cis-practice.ipynb` — your completed Jupyter notebook containing all code, plots, and written answers.

### Definition of done (checklist)

- [ ] `bootstrap_ci` function works correctly and is reusable (accepts any `stat_func`)
- [ ] Bootstrap CIs computed for mean, median, and proportion
- [ ] Normal-approximation CIs computed and compared in a summary table
- [ ] At least one polished visualisation of the bootstrap distribution with CI bounds
- [ ] Written interpretation paragraph in plain, non-technical language
- [ ] Notebook runs top-to-bottom without errors (`Kernel → Restart & Run All`)

### How to submit (Git workflow)

```bash
git add .
git commit -m "lab: bootstrap CIs in practice"
git push origin main
```

Then open a **Pull Request** from your fork back to the original repo. Paste the PR link in the student portal.
