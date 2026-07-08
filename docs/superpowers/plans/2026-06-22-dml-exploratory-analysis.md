# DML Exploratory Analysis Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a post-hoc exploratory DML layer for important SHAP/EBM features without modifying the prediction benchmark.

**Architecture:** Select 5–8 encoded, unscaled candidate columns whose source features are highly ranked and stable across SHAP, Conditional SHAP, and EBM. Because `econml` is unavailable, use five-fold cross-fitted CatBoost nuisance models to residualize outcome and treatment, then estimate adjusted effects with HC3-robust residual regression.

**Tech Stack:** Python, pandas, NumPy, CatBoost, scikit-learn, statsmodels, Matplotlib, Jupyter Notebook

---

### Task 1: Prepare DML candidates

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Add a failing structural test for DML variables.
- [ ] Select stable SHAP/Conditional SHAP/EBM source features.
- [ ] Expand original categorical features to encoded, unscaled columns.
- [ ] Exclude leakage fields and limit the set to eight columns.

### Task 2: Cross-fitted residualization DML

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Fit CatBoost outcome nuisance models within five folds.
- [ ] Fit CatBoost classifier/regressor treatment nuisance models by feature type.
- [ ] Regress residualized outcome on residualized treatment with HC3 inference.
- [ ] Create the required results schema and interpretation labels.

### Task 3: Save, plot, and verify

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Save the DML results table.
- [ ] Save the effect-summary figure with confidence intervals.
- [ ] Print positive, negative, and unclear signals.
- [ ] Execute against the actual training data and verify scope and artifacts.
