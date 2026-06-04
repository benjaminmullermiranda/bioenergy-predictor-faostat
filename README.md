# 🌱 Bioenergy Predictor — FAOSTAT

Supervised ML model to predict bioenergy production and consumption values
by country, year, item and measurement type, using public FAO data.

> **Team project** | My role: model training, hyperparameter optimization,
> and data leakage audit.

> **Deployment-ready**: includes `render.yaml` and Streamlit app configured
> for cloud deployment.

---

## 📊 Project Overview

| | |
|---|---|
| **Dataset** | 101,995 rows × 29 columns |
| **Source** | FAOSTAT (AF · AE · CISP · BE domains) |
| **Target variable** | Bioenergy value in terajoules (TJ) |
| **Model** | HistGradientBoostingRegressor |
| **Best R² (log-target)** | 0.990 (full) · **0.816 (control variant)** |
| **Deployment** | Streamlit · Render · React dashboard |

---

## 🤖 My Contribution — Model Training & Audit

### Model selection
Chose `HistGradientBoostingRegressor` for its native tolerance to null values,
efficiency on mixed tabular datasets, and ability to capture non-linear
relationships without manual transformations.

### Hyperparameter optimization
```python
GridSearchCV(
    estimator=HistGradientBoostingRegressor(),
    param_grid={
        'learning_rate': [0.05, 0.08, 0.1],
        'max_iter': [100, 140, 200],
        'max_leaf_nodes': [31, 50]
    },
    scoring='neg_root_mean_squared_error',
    cv=5
)
```
Best params: `learning_rate=0.08` · `max_iter=140` · `max_leaf_nodes=31`

### Data leakage audit
Detected anomalous R²=0.990 caused by temporal lag variables
(target_lag_1, target_lag_2, target_lag_3) with near-perfect correlation
to the target. Trained a control variant using only macroeconomic and
investment variables — R² dropped to 0.816, a realistic benchmark
for production scenarios.

| Variant | Predictors | R² | MAE (TJ) | Leakage risk |
|---|---|---|---|---|
| Full model | 26 | 0.990 | 60,528 | High |
| Control variant | 14 | 0.816 | 236,876 | Mitigated |

---

## 🔁 Reproducible Pipeline

```bash
# Clone and install
git clone https://github.com/benjaminmullermiranda/bioenergy-predictor-faostat
pip install -r requirements.txt

# Run full pipeline (download → features → DB → EDA → train)
python src/run_pipeline.py

# Launch Streamlit app
streamlit run src/app.py
```

---

## 🛠 Tech Stack

`Python` `scikit-learn` `pandas` `SQLite` `Streamlit` `Render`
`HistGradientBoosting` `GridSearchCV` `React`

---

## 👥 Team

Collaborative project developed as the final capstone of the
**4Geeks Academy Data Science & ML Bootcamp**.
Original repository: [Dragcessa1998/Proyecto-Final](https://github.com/Dragcessa1998/Proyecto-Final-4geeks-bioenerg-a-en-el-sector-agroalimentario)
