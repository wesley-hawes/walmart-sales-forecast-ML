# Walmart Weekly Sales Forecasting

This project focuses on **realistic weekly retail demand forecasting** using the Walmart Store Sales dataset.  
My objective was to learn more about time series forecasting from a business perspective. Additionally, I wanted to get a good understanding of my data through EDA. This helped with the iterrative process of improving my model based on EDA findings rather than just fitting and predicting blindly. 

Instead of optimizing for model complexity, this project focused more on:
- careful exploratory data analysis (EDA)
- leakage-aware feature engineering
- time-aware validation
- a clean and reproducible baseline forecasting pipeline
- learning past sales movement rather than simple time index


## Problem Statement

In practical retail forecasting settings, models must generate predictions using only information available up to the forecast date.  
The provided test set includes only `Store`, `Dept`, `Date`, and `IsHoliday`, which guided my approach to modeling:

In real world sales forecasting models can only use data up to the given forecast date. This was a challenge as I had to avoid leaking any information that the model could "peak into the future" with.
- avoids using forward-looking features
- uses lagged sales history as the primary signal to capture seasonality and lag dependence
- incorporates promotions and macro variable signals only through historical summaries when predicting



## Modeling Approach (Beating the baseline)

My modeling assumptions were built upon EDA findings

- **Sales structure**
  - Weekly sales were strong, non-stationary, and had sharo event driven deviations
  - Seasonal and holiday effects accounted for a large proportion of variance from the general sales trend over time

- **Lag features**
  - Short lags (1–2 weeks) were optimal due to weak residual autocorrelation found in EDA process
  - Small rolling intervals (4–8 weeks) 
  - Long autoregressive histories were avoided again due to weak residual autocorrelation

- **Promotions**
  - EDA found meaningful signal in Markdown missingness.
  - This prompted me to use missigness flagging and strategic imputation in the data pipeline. 
  - More specifically I used:
    - a promotion-present indicator to account for store/department cominations that varied in markdown frequenices.
    - imputation of numeric values (e.g., 0 when missing)

- **Store heterogeneity**
  - Store, Type, and Size were included to capture scale and behavioral difference signal

- **Validation**
  - Forward-chaining, time-based validation is used to reflect real forecasting conditions and prevent leakage

- **Random Search**
   - Used a custom random search algorithm to tune gradient boost hyperparameters in a time efficient manner



## Visualizations

Key plots from EDA and modeling are saved in the `assets/` directory:

- Sales trend and seasonality
- Autocorrelation behavior
- Promotion missingness patterns
- Validation predictions vs. actual values

These visuals complement the notebooks and highlight the main modeling insights.

**Baseline forecast vs. actuals on validation data**, illustrating how lagged sales and calendar effects capture overall demand patterns.
<img width="1479" height="593" alt="forecast_comparison" src="https://github.com/user-attachments/assets/d6ee1102-c5e2-4d51-9cec-15d1c5c71954" />


**Feature importance from the baseline model**, highlighting the dominance of lagged sales and calendar features.
<img width="1332" height="884" alt="feature_importance" src="https://github.com/user-attachments/assets/63cbb32e-a049-4a91-b644-34e6d96f8096" />



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


