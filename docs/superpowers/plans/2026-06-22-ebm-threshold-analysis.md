# EBM Threshold Analysis Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add an interpretable EBM analysis using the final CatBoost feature set, derive exploratory thresholds, and compare them with SHAP dependence thresholds.

**Architecture:** Map CatBoost-selected original features to the unscaled encoded branch, then fit a main-effects-only EBM with balanced sample weights. Extract global term scores directly from the EBM explanation API, derive thresholds with a documented score-crossing/largest-change heuristic, and create a SHAP comparison placeholder because numeric SHAP thresholds are not yet stored.

**Tech Stack:** Python, pandas, NumPy, scikit-learn, InterpretML EBM, Matplotlib, Jupyter Notebook

---

### Task 1: Prepare and fit EBM

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Add a failing structural check for EBM variables.
- [ ] Map selected CatBoost features to encoded, unscaled columns.
- [ ] Guard the section with an actionable `pip install interpret` message.
- [ ] Fit a main-effects EBM using balanced training sample weights.
- [ ] Save sanity metrics and confusion matrix.

### Task 2: Explain and extract thresholds

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Save EBM feature importance and global importance figure.
- [ ] Select SHAP-ranked and stable encoded features.
- [ ] Save one EBM curve per selected feature.
- [ ] Extract exploratory thresholds with direction and confidence labels.
- [ ] Save the threshold summary.

### Task 3: Compare with SHAP and verify

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Create the SHAP threshold comparison placeholder.
- [ ] Print all requested outputs.
- [ ] Execute the section against the actual dataset.
- [ ] Verify schemas, leakage exclusions, tables, and figures.
