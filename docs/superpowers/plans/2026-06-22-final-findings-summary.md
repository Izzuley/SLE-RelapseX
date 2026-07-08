# Final Findings Summary Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Consolidate completed model, explanation, threshold, DML, and sensitivity results into competition-ready summary tables and printed findings.

**Architecture:** Reformat existing result objects without retraining. Each subsection checks for its required inputs, creates the requested table when available, saves a CSV, and otherwise emits a clear warning while allowing later summaries to continue.

**Tech Stack:** Python, pandas, NumPy, Jupyter Notebook

---

### Task 1: Build final tables

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Add a failing structural test for all six final summary variables.
- [ ] Create benchmark and selected-predictor summaries.
- [ ] Create SHAP/Conditional SHAP and EBM threshold summaries.
- [ ] Create DML and remission sensitivity summaries.

### Task 2: Print competition-ready findings

**Files:**
- Modify: `Project Script/HCTM_Relapse_Project.ipynb`

- [ ] Print best-model and selected-feature details.
- [ ] Print explanation robustness and threshold findings.
- [ ] Print cautious DML and remission sensitivity findings.
- [ ] Print all required methodological cautions.

### Task 3: Verify artifacts

**Files:**
- Create at runtime: `outputs/tables/final_*.csv`

- [ ] Execute the summary section using existing saved results.
- [ ] Verify every required schema and CSV.
- [ ] Confirm no training or benchmark mutation occurs.
