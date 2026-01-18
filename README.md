# Walmart Weekly Sales Forecasting

This project focuses on **realistic weekly retail demand forecasting** using the Walmart Store Sales dataset.  
The objective is to predict future `Weekly_Sales` while respecting **real-world forecasting constraints**, where future promotions and macroeconomic conditions are not known at the time predictions are made.

Rather than optimizing for model complexity, this project emphasizes:
- careful exploratory data analysis (EDA)
- leakage-aware feature engineering
- time-based validation
- a clean, reproducible baseline forecasting pipeline

---

## Problem Context

In practical retail forecasting settings, models must generate predictions using only information available up to the forecast date.  
The provided test set includes only `Store`, `Dept`, `Date`, and `IsHoliday`, which motivates a modeling approach that:

- avoids forward-looking features
- uses lagged sales history as the primary signal
- incorporates promotions and macro variables only through historical summaries

This project is designed to mirror those constraints as closely as possible.

---

## Modeling Approach (Baseline)

Key modeling decisions are informed directly by EDA findings.

- **Sales structure**
  - Weekly sales exhibit a smooth long-term trend with sharp, event-driven deviations
  - Holiday effects explain a large portion of predictable variation

- **Lag features**
  - Short lags (1–2 weeks, optionally 4)
  - Small rolling windows (4–8 weeks)
  - Long autoregressive histories are avoided due to weak residual autocorrelation

- **Promotions**
  - MarkDown variables contain substantial, non-random missingness
  - Each MarkDown feature is represented using:
    - a promotion-present indicator
    - an imputed numeric value (e.g., 0 when missing)

- **Store heterogeneity**
  - Store, Type, and Size are included to capture scale and behavioral differences across stores

- **Validation**
  - Forward-chaining, time-based validation is used to reflect real forecasting conditions and prevent leakage

---

## Visualizations

Key plots from EDA and modeling are saved in the `assets/` directory for quick inspection, including:

- Sales trend and seasonality
- Autocorrelation behavior of detrended series
- Promotion missingness patterns
- Validation predictions vs. actuals

These visuals complement the notebooks and highlight the main modeling insights.

**Baseline forecast vs. actuals on validation data**, illustrating how lagged sales and calendar effects capture overall demand patterns.
<img width="1479" height="593" alt="forecast_comparison" src="https://github.com/user-attachments/assets/d6ee1102-c5e2-4d51-9cec-15d1c5c71954" />


**Feature importance from the baseline model**, highlighting the dominance of lagged sales and calendar features.
<img width="1332" height="884" alt="feature_importance" src="https://github.com/user-attachments/assets/63cbb32e-a049-4a91-b644-34e6d96f8096" />



---

## Repo Structure:
walmart-sales-forecast-ML/
│
├── notebooks/
│ ├── 01_eda.ipynb
│ │ - Exploratory analysis
│ │ - Trend and seasonality inspection
│ │ - Feature availability and modeling assumptions
│ │
│ ├── 02_baseline_model.ipynb
│ │ - Feature engineering
│ │ - Lagged sales baselines
│ │ - Time-based validation
│
├── src/
│ ├── features.py # Lag and rolling feature construction
│ ├── train.py # Model training utilities
│ ├── evaluate.py # Evaluation helpers
│
├── assets/
│ ├── example_plots.png # Selected visualizations for quick review
│
├── README.md
├── requirements.txt


> Raw data files are intentionally **not included
## How to Run

1. Obtain the Walmart Store Sales dataset and place it locally (data files are not included).
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
3. Run notebooks in order:

01_eda.ipynb

02_baseline_model.ipynb

Both notebooks are designed to run end-to-end from a clean kernel.


