# Feature Selection and Model Benchmarking Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the separate LR/CatBoost modelling flows with model-appropriate feature selection, four-model benchmarking, best-model preparation, and CSV outputs.

**Architecture:** L1 Logistic Regression selects scaled encoded columns, Boruta selects unscaled encoded columns, and BorutaSHAP selects original CatBoost columns using training data only. Encoded selections are mapped to original clinical feature groups for the MLP 2-of-3 consensus, then expanded back to scaled encoded columns. Each model is fitted once on its selected training data and evaluated on the shared hold-out test set.

**Tech Stack:** Python, pandas, scikit-learn, BorutaPy, BorutaShap, CatBoost, Jupyter Notebook

---

### Task 1: Replace legacy feature-selection flows

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Add a failing structural check for all required selected-feature variables.
- [ ] Implement L1 Logistic Regression selection on `X_train_ml`.
- [ ] Implement Boruta Random Forest selection on `X_train_tree`.
- [ ] Implement BorutaSHAP CatBoost selection on `X_train_cb`.
- [ ] Add encoded-to-original feature mapping for consensus voting.
- [ ] Create the MLP 2-of-3 consensus with union fallback.

### Task 2: Prepare datasets and benchmark models

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Create the four required selected train/test datasets.
- [ ] Fit LR, RF, MLP, and CatBoost using fixed lightweight configurations.
- [ ] Create `benchmark_results_df` and `confusion_matrices`.
- [ ] Create all best-model variables based primarily on test F1.
- [ ] Replace interpretation cells with a later-step placeholder.

### Task 3: Save and verify outputs

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`
- Create at runtime: `outputs/tables/*.csv`

- [ ] Save all four selected-feature CSV files.
- [ ] Save `model_benchmark_results.csv`.
- [ ] Verify leakage features are absent and feature selection uses training data only.
- [ ] Execute the section against the actual dataset and confirm required variables and files.
