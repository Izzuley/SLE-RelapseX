# Final CatBoost and SHAP Comparison Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Tune the selected CatBoost model, generate Standard and correlation-aware Conditional SHAP explanations, and compare their feature rankings.

**Architecture:** Tune only the BorutaSHAP-selected CatBoost features using training-fold F1. Compute Standard SHAP with CatBoost's native SHAP implementation. Approximate Conditional SHAP through exact Shapley aggregation over empirical nearest-neighbor conditional expectations, preserving joint feature patterns in the training data.

**Tech Stack:** Python, pandas, NumPy, scikit-learn, CatBoost, SHAP, Matplotlib, Jupyter Notebook

---

### Task 1: Final CatBoost tuning

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Add a failing structural test for final-model variables.
- [ ] Run compact five-fold F1 grid search using selected CatBoost features only.
- [ ] Evaluate the tuned model on the hold-out test set.
- [ ] Save metrics, parameters, and confusion matrix CSV files.

### Task 2: Standard SHAP

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Compute native CatBoost SHAP values.
- [ ] Create ranking, bar, beeswarm, and dependence outputs.
- [ ] Save the Standard SHAP ranking and figures.

### Task 3: Conditional SHAP approximation

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Implement empirical KNN conditional expectations for feature coalitions.
- [ ] Aggregate exact Shapley weights across all coalitions.
- [ ] Create ranking, bar, and dependence outputs.
- [ ] Document that the method is correlation-aware and not causal proof.

### Task 4: Compare explanation rankings

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Merge Standard and Conditional SHAP rankings.
- [ ] Calculate rank changes and interpretation labels.
- [ ] Save the comparison table and connected-rank figure.
- [ ] Execute all new sections against the actual dataset and verify every requested artifact.
