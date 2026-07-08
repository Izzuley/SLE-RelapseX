# REMISSION INTERVAL Sensitivity Analysis Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a secondary sensitivity branch that analyzes remission-duration thresholds without changing the leakage-safe primary model.

**Architecture:** Reload only `REMISSION INTERVAL` from the source workbook, align it to the existing split indices, and append it to copies of the final CatBoost-selected and encoded datasets. Fit separate CatBoost and additive EBM models, extract observed SHAP and EBM score-transition thresholds, and compare threshold proximity and risk direction.

**Tech Stack:** Python, pandas, NumPy, CatBoost, InterpretML EBM, Matplotlib, scikit-learn, Jupyter Notebook

---

### Task 1: Build the sensitivity branch

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Add a failing structural test for sensitivity variables.
- [ ] Reload only the remission-duration source column.
- [ ] Reuse the primary train/test indices.
- [ ] Fit remission imputation on training data only.
- [ ] Create CatBoost and encoded sensitivity datasets without `AGE RELAPSE`.

### Task 2: Fit and explain sensitivity models

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Fit a separate weighted CatBoost model using final compatible parameters.
- [ ] Save sensitivity metrics and confusion matrix.
- [ ] Compute REMISSION INTERVAL SHAP values, threshold, and orange-point dependence plot.
- [ ] Fit a separate additive EBM and save its remission curve and threshold.

### Task 3: Compare and verify

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Create the threshold comparison table.
- [ ] Save all required tables and figures.
- [ ] Execute against the actual dataset.
- [ ] Verify that the primary model, DML inputs, and benchmark remain unchanged.
