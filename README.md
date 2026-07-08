# SLE-RelapseX

SLE-RelapseX is a leakage-aware, explainable machine-learning workflow for
systemic lupus erythematosus relapse prediction.

The project compares logistic regression, random forest, multilayer perceptron,
and CatBoost models, then examines the selected model using standard SHAP,
correlation-aware Conditional SHAP, Explainable Boosting Machine thresholds,
and exploratory double machine learning.

## Main result

CatBoost achieved the strongest held-out performance:

- Precision: 0.714
- Recall: 0.625
- F1-score: 0.667

The principal predictors were lowest haemoglobin, 12-month C3 status, and
12-month anti-dsDNA status. SHAP and EBM identified a consistent exploratory
haemoglobin transition around 11.75–11.95.

These findings require prospective, multicentre validation and must not be
interpreted as definitive clinical cut-offs or causal proof.

## Repository structure

- `Project Script/` — complete analysis notebook
- `outputs/tables/` — aggregated result tables
- `outputs/figures/` — model interpretation figures
- `deliverables/` — competition abstract document
- `guide.md` — project analysis plan and methodological guidance

## Data availability

The patient-level source dataset is intentionally excluded from version
control for privacy and governance reasons. Place an authorised copy at:

`Dataset/ML_DATA SLE RELAPSE SSM.xlsx`

before running the notebook.

## Core dependencies

Python packages used include pandas, NumPy, SciPy, scikit-learn, CatBoost,
Boruta, BorutaShap, SHAP, InterpretML, EconML, statsmodels, and matplotlib.
