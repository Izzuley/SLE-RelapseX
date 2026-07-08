# Three Preprocessing Branches Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create CatBoost, unscaled tree/EBM, and scaled sklearn datasets from one leakage-safe train/test split.

**Architecture:** Split and impute once. Apply shared feature engineering and transformations, then derive three independent model-ready branches. Keep compatibility aliases so existing CatBoost and Logistic Regression cells remain runnable.

**Tech Stack:** Python, pandas, scikit-learn, CatBoost, Jupyter Notebook

---

### Task 1: Define and verify the three branches

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Add a failing structural check requiring `X_train_cb`, `X_train_tree`, and `X_train_ml`.
- [ ] Build the CatBoost branch with categorical values preserved as strings.
- [ ] Build the tree/EBM branch using one-hot encoding without scaling.
- [ ] Build the ML branch as a copy of the tree branch and fit `MinMaxScaler` on training data only.
- [ ] Preserve `X_train`/`X_test` and `X_train_lr`/`X_test_lr` aliases for current downstream cells.
- [ ] Verify equal row indices, aligned encoded columns, unscaled tree values, and scaled ML values.
