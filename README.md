# DengAI
DengAI – Pruned Time-Series Ensemble Baseline

This repository contains a feature-rich, time-aware regression pipeline for the DengAI: Predicting Disease Spread problem.
The goal is to predict weekly dengue case counts using climate, vegetation, and temporal signals while minimizing Mean Absolute Error (MAE).

📌 Problem Overview

Task: Predict total_cases (weekly dengue cases)

Type: Time-series regression

Evaluation Metric: Mean Absolute Error (MAE)

Cities:

IQ — Iquitos, Peru

SJ — San Juan, Puerto Rico

🧠 Modeling Strategy

This solution is built around strong feature engineering + per-city models + ensembling.

Key Ideas

Separate models per city (distinct climate dynamics)

Strict time-based validation (no leakage)

Heavy use of lagged, rolling, and exponential features

Ensemble of complementary regression models

🔧 Feature Engineering
Temporal Features

Month, quarter, day of year

Cyclical encodings:

sin/cos(weekofyear)

sin/cos(month)

Climate & Environmental Features

Temperature conversions (Kelvin → Celsius)

Temperature ranges

Humidity × precipitation interactions

NDVI aggregates (mean / min / max)

Total precipitation aggregates

Time-Series Features

For high-impact climate and NDVI variables:

Lag features: 1, 2, 4, 8, 12, 26, 52 weeks

Rolling means: 4, 12, 26 weeks

Exponentially weighted moving averages (EWM)

Categorical Encoding

One-hot encoding for city

Total engineered features: 247

🏗️ Models Used

Each city is modeled independently using a two-model ensemble:

1. Gradient Boosting Regressor

Loss: Absolute Error (MAE-optimized)

Deep trees with low learning rate

Strong at capturing non-linear interactions

2. Poisson Regressor

Suitable for count-based targets

Adds bias stability for low-count weeks

Ensemble Strategy

Predictions blended using inverse-MAE weighting

Weights learned on validation data per city

🔍 Validation Strategy

Time-aware split (last 52 weeks per city)

Validation size capped at 20% for smaller cities

No shuffling, no leakage

📊 Validation Results
Training for IQ (Iquitos)
GB MAE       : 8.1517
Poisson MAE  : 8.5452
Ensemble MAE : 8.2573

Training for SJ (San Juan)
GB MAE       : 7.4590
Poisson MAE  : 21.3573
Ensemble MAE : 10.2876

Best City Validation MAE
8.2573


⚠️ Note: Kaggle’s official score is computed on the hidden test set.
Validation MAE is used here as a reliable proxy.

🧪 Final Training & Inference

Models retrained on full city-specific training data

Median imputation applied consistently

Predictions clipped at 0 (no negative counts)

Outputs not rounded (empirically better MAE)

📁 Output

The pipeline produces:

submission.csv
├── id
└── total_cases


Fully compatible with the DengAI competition submission format.

▶️ How to Run
python main.py


Required files:

train.csv

test.csv

Dependencies:

pandas

numpy

scikit-learn

🚀 Future Improvements

LightGBM / CatBoost replacement for GB

City-specific hyperparameter tuning

Cross-validated ensembling

Neural sequence models (LSTM / Temporal CNN)

Feature selection via SHAP

🏁 Summary

This repository demonstrates:

Careful time-series handling

High-ROI feature engineering

Robust city-specific modeling

Competitive MAE-focused optimization

A strong baseline for DengAI and similar epidemiological forecasting problems.
