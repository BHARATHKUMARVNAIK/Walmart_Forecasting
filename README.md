# Walmart Sales Forecasting

## Overview

This project builds an end-to-end forecasting workflow for Walmart weekly sales using historical store-level sales data, calendar information, holiday effects, and macroeconomic variables.

The project covers:

- data cleaning
- exploratory data analysis
- advanced feature engineering
- rolling time-series cross-validation
- model tuning and benchmarking
- final model export for reuse or deployment

The final selected model is `Best CV LightGBM`.

## Problem Statement

The goal is to predict `Weekly_Sales` for Walmart stores as accurately as possible using past sales patterns and available external variables.

This can support:

- inventory planning
- staffing decisions
- holiday demand preparation
- supply-chain coordination
- store-level business forecasting

## Dataset

Source file:

- [data/Walmart.csv](/Users/bharathkumarvnaik/Downloads/programing/Data_Scientist_projects/Walmart_Forecasting/data/Walmart.csv)

Cleaned file:

- [data/Walmart_cleaned.csv](/Users/bharathkumarvnaik/Downloads/programing/Data_Scientist_projects/Walmart_Forecasting/data/Walmart_cleaned.csv)

Main columns in the dataset:

- `Store`
- `Date`
- `Weekly_Sales`
- `Holiday_Flag`
- `Temperature`
- `Fuel_Price`
- `CPI`
- `Unemployment`

Dataset summary:

- Rows: `6435`
- Stores: `45`
- Date range: `2010-02-05` to `2012-10-26`
- Holiday weeks: `450`
- Average weekly sales: `1,046,964.88`

## Project Structure

```text
Walmart_Forecasting/
├── data/
│   ├── Walmart.csv
│   ├── Walmart_cleaned.csv
│   └── external_business_drivers_template.csv
├── models/
│   ├── best_cv_lightgbm_metadata.json
│   └── best_cv_lightgbm_pipeline.joblib
├── notebooks/
│   ├── 1_cleaning.ipynb
│   ├── 2_EDA.ipynb
│   ├── 3_modeling.ipynb
│   └── 4_conclusion.ipynb
└── README.md
```

## Notebook Workflow

### 1. Data Cleaning

Notebook:

- [notebooks/1_cleaning.ipynb](/Users/bharathkumarvnaik/Downloads/programing/Data_Scientist_projects/Walmart_Forecasting/notebooks/1_cleaning.ipynb)

Main work done:

- parsed dates correctly
- checked missing values and duplicates
- validated business rules
- created calendar columns
- saved cleaned data for later stages

### 2. Exploratory Data Analysis

Notebook:

- [notebooks/2_EDA.ipynb](/Users/bharathkumarvnaik/Downloads/programing/Data_Scientist_projects/Walmart_Forecasting/notebooks/2_EDA.ipynb)

Main findings:

- strong sales variation across stores
- quarter 4 has the highest average sales
- holiday weeks show stronger sales behavior
- macro variables have weaker direct linear relationships than lag and calendar features

### 3. Advanced Modeling

Notebook:

- [notebooks/3_modeling.ipynb](/Users/bharathkumarvnaik/Downloads/programing/Data_Scientist_projects/Walmart_Forecasting/notebooks/3_modeling.ipynb)

Main modeling improvements:

- advanced lag features: `1, 2, 4, 8, 13, 26, 52`
- rolling mean and rolling standard deviation features
- cyclical seasonal features using sine and cosine transforms
- calendar edge features such as month start, month end, and weeks to quarter end
- rolling time-series cross-validation
- deeper tuning of `Extra Trees` and `LightGBM`
- final model export

### 4. Conclusion

Notebook:

- [notebooks/4_conclusion.ipynb](/Users/bharathkumarvnaik/Downloads/programing/Data_Scientist_projects/Walmart_Forecasting/notebooks/4_conclusion.ipynb)

This notebook summarizes the business findings, final model comparison, project limitations, and next steps.

## Final Model

Selected model:

- `Best CV LightGBM`

Saved artifacts:

- [models/best_cv_lightgbm_pipeline.joblib](/Users/bharathkumarvnaik/Downloads/programing/Data_Scientist_projects/Walmart_Forecasting/models/best_cv_lightgbm_pipeline.joblib)
- [models/best_cv_lightgbm_metadata.json](/Users/bharathkumarvnaik/Downloads/programing/Data_Scientist_projects/Walmart_Forecasting/models/best_cv_lightgbm_metadata.json)

Final holdout metrics:

- `RMSE`: `58164.80`
- `MAE`: `38162.16`
- `R2`: `0.9879`

Holdout cutoff date:

- `2012-06-22`

## Final Model Comparison

| Model | MAE | RMSE | R2 |
|---|---:|---:|---:|
| Best CV LightGBM | 38162.16 | 58164.80 | 0.9879 |
| Best CV Extra Trees | 37772.80 | 58457.01 | 0.9878 |
| Tuned Random Forest | 38523.18 | 60948.80 | 0.9867 |
| Linear Regression | 53984.94 | 73564.94 | 0.9807 |
| Naive Lag-1 Baseline | 52680.98 | 80073.12 | 0.9771 |

## Feature Set Used by the Final Model

The final model uses `39` engineered features, including:

- lag features
- rolling mean features
- rolling standard deviation features
- cyclical time features
- calendar structure features
- store and holiday information

Examples:

- `lag_1`, `lag_4`, `lag_13`, `lag_52`
- `rolling_mean_4`, `rolling_mean_13`, `rolling_std_26`
- `Week_sin`, `Week_cos`, `Month_sin`, `Month_cos`
- `Year_Index`, `Weeks_To_Quarter_End`
- `sales_diff_1`, `sales_ratio_4_13`, `sales_ratio_4_26`

## External Business Drivers

The project now supports external business variables such as:

- promotions
- markdown spend
- discount percentage
- competitor promotions
- local events

Template file:

- [data/external_business_drivers_template.csv](/Users/bharathkumarvnaik/Downloads/programing/Data_Scientist_projects/Walmart_Forecasting/data/external_business_drivers_template.csv)

If you add a real file named `external_business_drivers.csv` in the `data` folder, the modeling notebook is set up to merge it automatically by `Store` and `Date`.

Current status:

- external business-driver columns used in the final saved model: `0`

## How To Run

1. Open the notebooks in order:
   `1_cleaning.ipynb` -> `2_EDA.ipynb` -> `3_modeling.ipynb` -> `4_conclusion.ipynb`
2. Run all cells in sequence.
3. If you have promotion or markdown data, place it at:
   `data/external_business_drivers.csv`
4. Re-run the modeling notebook to retrain and resave the final model.

## Basic Requirements

This project uses Python with common data science libraries, including:

- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`
- `scikit-learn`
- `lightgbm`
- `joblib`

## Business Value

This forecasting pipeline can help Walmart:

- improve weekly inventory planning
- reduce stockouts and overstocking
- prepare better for holiday demand spikes
- make store-level decisions with stronger forecasts
- move toward a reusable deployment-ready prediction workflow

## Limitations

- no real promotions or markdown dataset was available yet
- forecasting is still at store-week level, not product or department level
- no multi-horizon backtesting dashboard has been built yet
- local events and competitor effects are only supported if external data is provided

## Next Steps

- add real promotion and markdown data
- extend to department-level or product-level forecasting
- build a prediction script or API around the saved model
- add monitoring and backtesting for production-style usage
- tune `LightGBM` further if more business-driver data becomes available

## Authoring Note

This README reflects the latest advanced-modeling version of the project, where `Best CV LightGBM` replaced earlier model winners after rolling cross-validation and richer feature engineering.
