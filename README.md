# mlops-training-data

Public datasets used by the [VersoML MLOps training](https://github.com/VersoML/mlops-training).

## Datasets

| File | Rows | Purpose |
|------|------|---------|
| `churn.parquet` | ~1 000 | Synthetic SaaS churn dataset used in modules 1–3 to train and serve `churn-model`. |

### Schema — `churn.parquet`

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

## Raw URLs

```
https://raw.githubusercontent.com/VersoML/mlops-training-data/main/churn.parquet
```
