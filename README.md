# UK-Electricity-Demand-Forecasting

This project builds a complete end to end forecasting pipeline for UK national electricity demand using real historical data. It combines half hourly ESO demand data for years 2022 to 2024 with hourly temperature data from the NOAA Global Hourly (METAR) dataset. After cleaning and merging the datasets, the project engineers a rich feature set and trains multiple forecasting models, evaluating them on an unseen 2024 test set.

## Data sources

• ESO Historic Demand Data (2022 to 2024)  
• NOAA Global Hourly temperature data for London Heathrow (EGLL)

The raw CSVs are stored under `data/raw` and the cleaned merged dataset is stored under `data/processed`.

## Feature engineering

The pipeline builds all standard features used in professional electricity demand forecasting:

• Temperature and temperature lags  
• Demand lags (1, 2, 3, 48, 49, 96 half hours)  
• Hour of day  
• Day of week  
• Month  
• Weekend flag  
• UK bank holiday indicator (holidays library)

All lag-created missing rows are dropped.

## Train and test split

To avoid leakage, the split is strictly time based:

• Train: 2022 to 2023  
• Test: 2024 (unseen)

This simulates a real forecasting scenario where the model must predict a future year.

## Models

The project includes several models:

### Naive baseline (lag 48)
• MAE about 1889  
• RMSE about 2628  

### Linear Regression
• MAE about 258  
• RMSE about 345  
• Coefficients provide insight into temperature sensitivity and seasonality

### Ridge Regression
• Same performance as Linear Regression  
• Confirms low multicollinearity in the feature set

### Random Forest Regression
• Best performance  
• MAE about 255  
• RMSE about 339  
• Strongest feature is lag 1 (autocorrelation)

## Evaluation

Plots generated include:

• Actual versus predicted (zoomed)  
• Scatter accuracy plot  
• Residual plot  

These confirm strong autocorrelation, seasonality patterns, temperature effects, and weekend or holiday behaviour.
