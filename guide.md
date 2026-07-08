# SLE-RelapseX Pipeline Changes

Here’s what needs to change, step by step.

## Step-by-step changes

### 1. Rename project framing

From:

> CatBoost-based SLE relapse prediction

To:

> **SLE-RelapseX: Explainable and Causality-Aware ML for SLE Relapse Prediction**

Goal shifts from only prediction to:

```text id="rcg5xx"
Prediction → Stable predictors → Explanation robustness → Thresholds → Causal signals
```

---

### 2. Fix leakage policy

Add a section early in the notebook:

```text id="7be7xb"
Leakage Review and Feature Policy
```

Main model must exclude:

```text id="0qfo52"
REMISSION INTERVAL
AGE RELAPSE
```

Use **REMISSION INTERVAL only in sensitivity analysis**, not the main model.

---

### 3. Fix preprocessing order

Current issue:

```text id="ae61yv"
Imputation happens before train-test split
```

Change to:

```text id="i27p9j"
Train-test split first
↓
Fit imputer on training set only
↓
Apply imputer to test set
```

This prevents preprocessing leakage.

---

### 4. Create two clean data versions

You need:

| Dataset version | Used for |
|---|---|
| CatBoost-ready data | CatBoost, BorutaSHAP, SHAP |
| sklearn-ready encoded/scaled data | LR, RF, MLP, EBM, DML |

---

### 5. Replace old SHAP feature selection

Replace:

```text id="6fwfc7"
CatBoost → SHAP top features
```

With:

```text id="vdr9qs"
BorutaSHAP on training data only
```

Prefer repeated/stability BorutaSHAP because n = 120.

---

### 6. Add model benchmarking

Currently only LR and CatBoost.

Add:

```text id="4g2mv7"
Logistic Regression
Random Forest
MLP
CatBoost
```

Report:

```text id="atep8v"
Precision
Recall
F1-score
Confusion matrix
```

---

### 7. Keep CatBoost as final/main model

After benchmarking, choose final CatBoost if it performs best or is clinically most suitable.

Then tune final CatBoost using BorutaSHAP-selected features.

---

### 8. Improve Standard SHAP section

Keep standard SHAP, but organize outputs:

```text id="qlf0bl"
SHAP bar plot
SHAP beeswarm plot
SHAP dependence plots
```

This is your baseline XAI.

---

### 9. Add Conditional SHAP

Immediately after standard SHAP, add:

```text id="xz5zi5"
Conditional SHAP bar plot
Conditional SHAP dependence plots
```

Purpose:

> Check whether feature importance changes when correlated clinical variables are considered.

---

### 10. Add SHAP comparison table

Create:

| Feature | Standard SHAP rank | Conditional SHAP rank | Interpretation |
|---|---:|---:|---|
| Feature A |  |  | Stable/changed |

This is one of your strongest competition outputs.

---

### 11. Add EBM threshold analysis

After SHAP dependence plots, train EBM to find readable thresholds.

Compare:

| Feature | SHAP threshold | EBM threshold | Agreement |
|---|---|---|---|

Purpose:

> Juxtapose SHAP dependence thresholds with EBM risk curves.

---

### 12. Add DML exploratory causal analysis

After EBM, run DML feature-by-feature:

```text id="gwiahs"
Treatment = important feature
Outcome = relapse
Covariates = other selected features
```

Use careful wording:

> DML explores possible causal signals, not proof of causality.

---

### 13. Add REMISSION INTERVAL sensitivity analysis

Near the end:

```text id="70xz1t"
Sensitivity Analysis: REMISSION INTERVAL
```

Use it only for:

```text id="r8rw0g"
SHAP dependence threshold
EBM threshold
clinical discussion
```

Not for main model performance.

---

### 14. Save final outputs

Save:

```text id="0ik30v"
metrics table
confusion matrices
BorutaSHAP selected features
standard SHAP plots
Conditional SHAP plots
SHAP comparison table
EBM plots
EBM threshold table
DML results table
REMISSION INTERVAL sensitivity plots
abstract-ready findings
```

This matches the implementation map already saved in your project source. fileciteturn3file2

# Notebook enhancement roadmap

Yes — don’t automate everything yet. Build it **step-by-step in the notebook**, so you understand each method and can defend it.

Below is the best implementation map based on your current relapse notebook.

## Current notebook structure

Your notebook currently looks like this:

```text id="s64lg1"
1. Load Data
2. Preprocessing
3a. Logistic Regression + RFE + SHAP
3b. CatBoost + SHAP feature selection + modelling + SHAP interpretation
4. Save Model
```

The enhancement should become:

```text id="eikyzg"
1. Load Data
2. Leakage check + feature policy
3. Preprocessing
4. BorutaSHAP feature selection
5. Model benchmarking: LR, RF, MLP, CatBoost
6. Final CatBoost model
7. Standard SHAP
8. Conditional SHAP
9. Standard SHAP vs Conditional SHAP comparison
10. EBM threshold analysis
11. DML exploratory causal analysis
12. Sensitivity analysis with REMISSION INTERVAL
13. Save outputs
```

## Very important fix first

In your current notebook, this happens:

```python id="t9sxj3"
data = impute_missing_values(data, numeric_cols, categorical_cols)
X_train, X_test, y_train, y_test = stratified_split(data, target_col)
```

That means imputation is done **before train-test split**.

For the enhanced version, fix this first:

```text id="fx8new"
Split first → fit imputation on training set → apply to test set
```

Otherwise, the test set leaks information into training preprocessing.

## Where each modern technique goes

| Technique | Where to insert | Replace or add? | Purpose |
|---|---|---|---|
| Leakage policy | After `get_feature_columns()` | Add new section | Decide main vs sensitivity features |
| BorutaSHAP | After preprocessing, before model benchmarking | Replace current CatBoost SHAP feature selection | Select stable predictive features |
| RF + MLP benchmark | After BorutaSHAP | Add new section | Diversify model comparison |
| Standard SHAP | After final CatBoost tuning | Keep and improve current SHAP section | Baseline XAI |
| Conditional SHAP | Immediately after Standard SHAP | Add new section | Correlation-aware comparison |
| SHAP comparison table | After both SHAP methods | Add new section | Show changed/stable feature rankings |
| EBM | After SHAP dependence plots | Add new section | Validate thresholds in readable risk curves |
| DML | After EBM | Add new section | Explore possible causal signals |
| REMISSION INTERVAL sensitivity | Separate branch near end | Add new section | Clinical-threshold analysis, not main model |

## Specific notebook placement

### 1. After Cell 10: add leakage policy

Your Cell 10 defines numeric and categorical columns. Add a new markdown/code cell immediately after it:

```text id="dwqn9w"
# 2a. Leakage Review and Feature Policy
```

In that section, define:

```python id="8lbnph"
target_col = "RELAPSE1,NONRELAPSE0"

potential_leakage_features = [
    "REMISSION INTERVAL",
    "AGE RELAPSE"   # also suspicious if calculated from relapse event
]

primary_excluded_features = potential_leakage_features

sensitivity_features = [
    "REMISSION INTERVAL"
]
```

Main model:

```python id="dk7evu"
X_main = data.drop(columns=[target_col] + primary_excluded_features)
```

Sensitivity model:

```python id="ynjwtu"
X_sensitivity = data.drop(columns=[target_col])
```

This gives you two clean branches:

```text id="z76wwb"
Main predictive model = no leakage features
Sensitivity explanation = includes REMISSION INTERVAL
```

---

### 2. Replace Cells 24 and 26 with one clean preprocessing pipeline

Currently you have separate CatBoost and LR preprocessing blocks. Better to create two outputs from one split:

```text id="ckwxj5"
# 2b. Split first
# 2c. Impute using training data only
# 2d. Feature engineering
# 2e. Encoded/scaled version for LR, RF, MLP
# 2f. CatBoost version with categorical columns
```

You need two dataset versions:

| Dataset version | Used by |
|---|---|
| `X_train_cb`, `X_test_cb` | CatBoost + BorutaSHAP + SHAP |
| `X_train_ml`, `X_test_ml` | LR + RF + MLP + EBM/DML if encoded |

Do not keep separate duplicated preprocessing for LR and CatBoost unless necessary.

---

### 3. Replace Cells 43–48 with BorutaSHAP

Your current CatBoost feature selection is:

```text id="00xh6a"
Train CatBoost on all features
→ compute SHAP
→ keep top SHAP features
```

Replace that with:

```text id="s9cpn8"
# 3. BorutaSHAP Feature Selection
```

This goes **after preprocessing** and **before all model benchmarking**.

Important rule:

```text id="ttygai"
Run BorutaSHAP only on X_train, never on X_test.
```

Output should be:

```python id="2h2ojj"
selected_features = [...]
X_train_cb_sel = X_train_cb[selected_features]
X_test_cb_sel = X_test_cb[selected_features]

X_train_ml_sel = X_train_ml[selected_features_encoded_or_mapped]
X_test_ml_sel = X_test_ml[selected_features_encoded_or_mapped]
```

For now, simplest approach:

```text id="5zn3en"
Run BorutaSHAP on CatBoost-ready training data.
Use selected feature names as the common feature set.
Then encode/scale those selected features for LR/RF/MLP.
```

---

### 4. Insert model benchmarking after BorutaSHAP

This should come before your final CatBoost SHAP section.

Add:

```text id="qfcr53"
# 4. Model Benchmarking
```

Models:

```text id="77x27i"
Logistic Regression
Random Forest
MLP
CatBoost
```

Report only:

```text id="u4vgr3"
Precision, Recall, F1, Confusion Matrix
```

Use the same selected features for all models.

Your benchmarking table should look like:

| Model | Precision | Recall | F1 | Notes |
|---|---:|---:|---:|---|
| LR |  |  |  | Baseline |
| RF |  |  |  | Tree ensemble |
| MLP |  |  |  | Neural benchmark |
| CatBoost |  |  |  | Main model |

---

### 5. Keep Cell 51 as final CatBoost tuning, but update input

Your Cell 51 currently tunes CatBoost using:

```python id="xdlrhr"
X_train_selected
X_test_selected
```

Keep the structure, but make sure those features now come from **BorutaSHAP**, not the old SHAP top-k method.

So rename clearly:

```python id="f9fkju"
X_train_cb_boruta = X_train_cb[selected_features]
X_test_cb_boruta = X_test_cb[selected_features]
```

Then tune:

```python id="jffgmj"
grid_search.fit(X_train_cb_boruta, y_train, cat_features=cat_features_selected)
```

---

### 6. Keep Cells 53–54 as Standard SHAP

Your Cells 53–54 are already the right location for standard SHAP.

Rename the section:

```text id="cs2eb9"
# 6. Standard SHAP Interpretation
```

Produce:

```text id="qjwsjw"
Standard SHAP bar plot
Standard SHAP beeswarm plot
Standard SHAP dependence plots
```

Use the final CatBoost model.

---

### 7. Add Conditional SHAP immediately after Cell 54

Add new section:

```text id="ep0zig"
# 7. Conditional SHAP Interpretation
```

Purpose:

```text id="cc2jp5"
Compare whether feature importance changes when feature dependence/correlation is considered.
```

Output:

```text id="n6cv5y"
Conditional SHAP bar plot
Conditional SHAP dependence plots
Feature ranking comparison with standard SHAP
```

Important phrasing:

> Conditional SHAP is used as a correlation-aware explanation method, not as causal proof.

---

### 8. Add SHAP comparison after Conditional SHAP

Add:

```text id="qsh159"
# 8. Standard SHAP vs Conditional SHAP Comparison
```

Create a table:

| Feature | Standard SHAP Rank | Conditional SHAP Rank | Interpretation |
|---|---:|---:|---|
| LOWEST HB |  |  | Stable / changed |
| C3 LEVEL |  |  | Stable / changed |
| IMMUNOSUPPRESSANT |  |  | Stable / changed |

This is one of your strongest competition outputs.

---

### 9. Add EBM after SHAP dependence plots

Add:

```text id="3jzpkx"
# 9. EBM Threshold Analysis
```

Use EBM to find readable threshold patterns.

Best use:

```text id="q5ovf7"
Use EBM on original-scale/imputed features where possible.
```

Why? Because if you use Box-Cox/log transformed values, your threshold becomes harder to explain clinically.

Use EBM to answer:

```text id="pkhne4"
At what value of LOWEST HB / C3 LEVEL / LOWEST LYMP / LOWEST NEUT does relapse risk appear to increase?
```

Then compare:

| Feature | SHAP dependence threshold | EBM threshold | Agreement |
|---|---|---|---|
| LOWEST HB |  |  | Yes/No |
| C3 LEVEL |  |  | Yes/No |
| LOWEST LYMP |  |  | Yes/No |

---

### 10. Add DML after EBM

Add:

```text id="eka85w"
# 10. Exploratory DML Causal Analysis
```

DML should not be used for prediction. It is a separate analysis layer.

For each important feature:

```text id="4xig7j"
Treatment = one important feature
Outcome = relapse
Covariates = remaining selected features
```

Example:

```text id="9aqygc"
Treatment: LOWEST HB
Outcome: RELAPSE1,NONRELAPSE0
Covariates: C3 LEVEL, LOWEST LYMP, LOWEST NEUT, IMMUNOSUPPRESSANT, etc.
```

Output:

| Treatment feature | Estimated effect | Direction | Interpretation |
|---|---:|---|---|
| LOWEST HB |  | Protective/risk | Exploratory only |
| C3 LEVEL |  | Protective/risk | Exploratory only |
| INFECTION YES1 |  | Risk | Exploratory only |

Wording must be careful:

> DML was used to explore possible causal signals after adjustment for observed covariates, not to prove causality.

---

### 11. Add REMISSION INTERVAL sensitivity near the end

Add:

```text id="rnsdxl"
# 11. Sensitivity Analysis: REMISSION INTERVAL
```

This is where you include it.

Do **not** put it in the main model.

Use it for:

```text id="vigmsn"
SHAP dependence threshold
EBM threshold
Clinical discussion
```

Best logic:

```text id="w8zt8o"
Primary model excludes REMISSION INTERVAL due to possible outcome-timing leakage.
Sensitivity analysis includes it to explore whether remission duration shows clinically meaningful risk thresholds.
```

That is methodologically strong.

## Final recommended notebook order

```text id="s8hfkp"
1. Load Data

2. EDA + DQR

3. Leakage Review
   - Exclude REMISSION INTERVAL from main model
   - Keep for sensitivity analysis only

4. Train/Test Split
   - Split before imputation

5. Preprocessing
   - Impute train → apply to test
   - Feature engineering
   - Encode/scale for LR/RF/MLP
   - CatBoost categorical handling

6. BorutaSHAP Feature Selection
   - Training data only
   - Stable selected features

7. Model Benchmarking
   - LR
   - RF
   - MLP
   - CatBoost

8. Final CatBoost Tuning

9. Standard SHAP
   - Bar plot
   - Beeswarm
   - Dependence plots

10. Conditional SHAP
   - Bar plot
   - Dependence plots

11. SHAP vs Conditional SHAP Comparison
   - Ranking table
   - Interpretation

12. EBM Threshold Analysis
   - Risk curves
   - Threshold table

13. DML Exploratory Causal Analysis
   - Feature-by-feature causal signal check

14. REMISSION INTERVAL Sensitivity Analysis
   - SHAP/EBM threshold only

15. Save Outputs
   - Metrics table
   - Confusion matrices
   - SHAP plots
   - EBM plots
   - DML table
```

## Best immediate next step

Start by fixing the notebook structure, not coding the advanced methods yet.

First Codex VS Code prompt should be:

```text id="d0yahs"
Refactor this notebook so that preprocessing is split-safe. 
Do not change modelling yet. 
Move train-test split before imputation. 
Create two clean datasets:
1. CatBoost-ready data with categorical columns preserved.
2. sklearn-ready data with categorical columns encoded and numeric columns scaled.
Also add a leakage policy section that excludes REMISSION INTERVAL and AGE RELAPSE from the primary model, while keeping REMISSION INTERVAL for later sensitivity analysis.
```

That is the correct first move.
