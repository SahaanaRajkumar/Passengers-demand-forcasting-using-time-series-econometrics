
# AirPassengers Forecasting using SARIMA

## Overview

This project implements an end-to-end time series forecasting pipeline using the classical **AirPassengers** dataset. Rather than simply fitting a SARIMA model, the notebook follows a systematic workflow that combines statistical time series modelling with modern forecasting validation techniques.

The objective is to forecast monthly airline passenger demand while accounting for long-term trend, changing variance, and yearly seasonality.

---

## Dataset

The AirPassengers dataset contains the monthly number of international airline passengers from **January 1949 to December 1960** (144 observations).

Key characteristics of the dataset include:

- Upward long-term trend
- Increasing variance over time
- Strong yearly seasonality (12-month cycle)

These characteristics make it an ideal dataset for demonstrating seasonal time series forecasting.

---

## Project Workflow

The notebook follows the complete forecasting workflow below:

1. Load and visualize the dataset
2. Split the data into training and testing sets
3. Apply log transformation to stabilize variance
4. Perform stationarity testing using the Augmented Dickey-Fuller (ADF) test
5. Determine regular (`d`) and seasonal (`D`) differencing orders
6. Examine ACF and PACF plots
7. Perform SARIMA hyperparameter tuning using grid search
8. Compare candidate models using AIC and BIC
9. Evaluate forecasting performance on unseen test data
10. Select the best-performing model
11. Retrain the selected model on the complete dataset
12. Forecast future passenger demand

---

## Why a Train-Test Split?

Many introductory SARIMA tutorials fit the model using the entire dataset before generating forecasts.

In practice, this makes it impossible to evaluate how well the model performs on unseen observations.

To obtain an unbiased estimate of forecasting performance, the final **24 months** were reserved as a test set while the remaining observations were used for model training.

This mirrors real-world forecasting, where future observations are unavailable during model development.

---

## Stationarity

SARIMA assumes that the input series is stationary.

Because the original AirPassengers series contains both trend and seasonality, stationarity was assessed after progressively applying:

- Log transformation
- First-order differencing
- Seasonal differencing
- Combined first and seasonal differencing

The Augmented Dickey-Fuller (ADF) test confirmed that stationarity was achieved only after applying both regular and seasonal differencing, leading to:

- Regular differencing (`d`) = **1**
- Seasonal differencing (`D`) = **1**
- Seasonal period (`s`) = **12**

---

## Model Selection

Candidate SARIMA models were generated using a grid search across multiple combinations of:

- Non-seasonal AR order (`p`)
- Non-seasonal MA order (`q`)
- Seasonal AR order (`P`)
- Seasonal MA order (`Q`)

Models that failed to converge during estimation were discarded.

The remaining models were ranked using:

- Akaike Information Criterion (AIC)
- Bayesian Information Criterion (BIC)

Rather than selecting the model solely on the basis of in-sample fit, the best-performing candidates were further evaluated on unseen test data.

---

## Forecast Evaluation

Forecast accuracy was assessed using:

- **Mean Absolute Error (MAE)** – average magnitude of forecasting errors.
- **Root Mean Squared Error (RMSE)** – penalizes larger forecasting errors more heavily.
- **Mean Absolute Percentage Error (MAPE)** – average percentage forecasting error.

Unlike AIC and BIC, these metrics evaluate predictive performance on observations that were not used during model training.

The final model was therefore selected based on its ability to generalize to unseen data rather than solely on in-sample goodness of fit.

---

## Final Model

The selected model was retrained using the complete dataset before generating future forecasts.

Retraining allows the model to utilize all available historical information while retaining the SARIMA specification identified during model selection.

---

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Statsmodels
- Scikit-learn

---



## Key Learning Outcomes

This project demonstrates:

- Time series preprocessing
- Stationarity testing
- Trend and seasonal differencing
- SARIMA model development
- Hyperparameter tuning using grid search
- Forecast validation using train-test evaluation
- Model comparison using statistical and predictive metrics
- Future demand forecasting

---

## Future Improvements

Potential extensions include:

- Rolling-origin (walk-forward) time series cross-validation
- Automatic model selection using AutoARIMA
- Comparison with Holt-Winters Exponential Smoothing
- Comparison with Prophet
- Comparison with machine learning and deep learning forecasting models such as XGBoost and LSTM



## Results

Among the candidate SARIMA models evaluated, the final selected model achieved:

| Metric | Value |
|---------|------:|
| MAE | 38.61 |
| RMSE | 41.85 |
| MAPE | 8.54% |

The model successfully captured both the long-term trend and annual seasonal pattern, achieving good forecasting accuracy on the unseen 24-month test set.
