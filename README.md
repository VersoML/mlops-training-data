# mlops-training-data

Public datasets and model artifact used by the [VersoML MLOps training](https://github.com/VersoML/mlops-training).

## Datasets

| File | Rows | Purpose |
|------|------|---------|
| `churn.parquet` | 10 000 | Synthetic SaaS churn dataset used in modules 1–3 to train and serve `churn-model`. Also the **reference** batch for monitoring. |
| `churn_drifted.parquet` | 10 000 | Drifted variant of `churn.parquet` used as the **current** batch in module 4 (observability / drift detection). Same rows and labels, shifted feature distributions. |
| `reference_scored.parquet` | 10 000 | `churn.parquet` scored by `model.joblib` (adds a `prediction` column). Reference input for the Evidently reports/tests in module 4. |
| `current_scored.parquet` | 10 000 | `churn_drifted.parquet` scored by `model.joblib` (adds a `prediction` column). Current input for the Evidently reports/tests in module 4. |

## Model

| File | Purpose |
|------|---------|
| `model.joblib` | Pre-trained scikit-learn `Pipeline` (`ColumnTransformer(OneHotEncoder(plan) + passthrough)` → `MLPClassifier(hidden_layer_sizes=(50,), solver="lbfgs", max_iter=100, random_state=42)`). Predicts `churned` from the 7 feature columns; `classes_ = [False, True]`. Served directly via the KServe `storageUri`, and used to score the `*_scored.parquet` datasets above. |

## Schema — `churn.parquet` / `churn_drifted.parquet`

| Column | Type | Notes |
|--------|------|-------|
| `tenure_days` | int | Days since signup |
| `nb_logins_30j` | int | Logins in the last 30 days |
| `nb_features_used` | int | Distinct features used |
| `plan` | str | `Free` \| `Pro` \| `Enterprise` |
| `support_tickets_90j` | int | Support tickets in last 90 days |
| `mrr_eur` | float | Monthly recurring revenue (EUR) |
| `has_integration` | bool | Has at least one 3rd-party integration |
| `churned` | bool | Target — did the customer churn |

The `*_scored.parquet` files add one column:

| Column | Type | Notes |
|--------|------|-------|
| `prediction` | int | `model.joblib` prediction (0 / 1) |

### Drift in `churn_drifted.parquet`

Same 10 000 rows and identical `churned` labels (~11.1% churn); only the input features are shifted, to exercise data-drift detection:

- `mrr_eur` mean ≈ 64 → 106
- `nb_logins_30j` mean ≈ 7.8 → 8.4
- `has_integration` rate ≈ 0.29 → 0.40
- `plan` mix flips: `Free`-majority → `Pro`-majority (`Enterprise` roughly unchanged)
- `tenure_days`, `nb_features_used`, `support_tickets_90j` unchanged

## Raw URLs

```
https://raw.githubusercontent.com/VersoML/mlops-training-data/main/churn.parquet
https://raw.githubusercontent.com/VersoML/mlops-training-data/main/churn_drifted.parquet
https://raw.githubusercontent.com/VersoML/mlops-training-data/main/reference_scored.parquet
https://raw.githubusercontent.com/VersoML/mlops-training-data/main/current_scored.parquet
https://raw.githubusercontent.com/VersoML/mlops-training-data/main/model.joblib
```
