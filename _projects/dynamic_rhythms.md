---
layout: page
title: Dynamic Rhythms
description: Time series prediction challenge for power outages (5th place)
img: assets/img/dynamic_rhythms/icon_image.png
importance: 3
category: competitions
---

## Challenge Overview

Predicting power outages, especially those linked to extreme weather, is crucial for public safety, economic stability, and emergency response. I recently tackled this complex problem in the [**Dynamic Rhythms** challenge](https://thinkonward.com/app/c/challenges/dynamic-rhythms), aiming to build robust models for anticipating grid disruptions.

The task was to develop a reliable system for predicting power outages and their correlation with rare and extreme weather events. Key challenge aspects included:

- Dual data sources: The model needed to integrate these distinct yet interconnected sources.
- Unstructured evaluation: Train/test/hold-out sets and metrics were not fixed but could be customized, encouraging innovative approaches.
- Holistic prediction: The goal was to predict if, where, and when outages would happen, with sufficient lead time, and to forecast their severity and duration.

## Exploratory Data Analysis Insights

A deep understanding of the data's inherent characteristics was paramount. Our analysis of the aggregated daily, state-level data revealed three critical insights that directly informed our modeling strategy:

1. **Extreme Target Skewness**: The target variable, customers without power (customers_out), exhibited extreme skewness (Median: 236 vs. Max: >5.3 million). This confirmed the need for a **log transformation on the target** (log(1 + customers_out)) to stabilize variance and allow gradient boosting models to learn from the bulk of the data, rather than being solely driven by massive, rare outliers.
2. **Event Rarity and Sparsity**: Many event types were exceptionally rare (e.g., Tsunami, Volcanic Ashfall, 94%+ missing). Our strategy was to impute all missing event counts with zero (0) and create binary indicator features for these rare events to capture their presence when they did occur, even if their count was small.
3. **Correlation Blueprint**: Correlation analysis highlighted the "grid killers": **Tropical Storms (0.24)** and **Hurricanes (0.18)** showed the strongest correlations with outages. This prioritized features derived from these events.

## Proposed Solution

The core of our approach was a meticulous feature engineering pipeline designed to translate the spatio-temporal dynamics into meaningful signals for the chosen models, **XGBoost** and **LightGBM**.

### Robust Feature Engineering

We engineered features across six key categories to capture different facets of the problem:

- **Event Count & Indicators**: We used the raw event counts directly, and for rare events ( $>95\%$ zero counts), we created is_event_type binary indicators to capture occurrence.
- **Dominant Event**: A categorical feature, dominant_event, was created to capture which storm event had the highest count on a given day, providing a clear signal of the primary stressor.
- **Interaction Terms**: Based on domain knowledge, we created multiplicative interaction terms (e.g., ice_storm_winter_storm_interaction) to model the combined, synergistic effect of multiple concurrent weather conditions.
- **Drought Handling**: Given the anomalous, continuous nature of the event_count Drought, we used both the raw value and a log-transformed version (log_drought_index) to capture its non-linear effect on long-term grid vulnerability.
- **Temporal Features**: Standard features (year, month, day_of_week, day_of_year) captured essential seasonality.
- **Lagged & Window Features (Lead-Time)**: To achieve forecasting lead-time, we created lagged event counts (1, 2, and 7 days) for high-impact events (Hurricane, Tropical Storm, Winter Storm). Additionally, maximum event counts over 3 and 7-day rolling windows were computed to capture periods of overall weather intensity.

### Model Architecture and Training

We chose XGBoost and LightGBM for their robustness, speed, and ability to handle non-linear relationships in structured data.

- **Training Target**: The model was trained on the log-transformed target (customers_out_log).
- **Temporal Split**: We used a rigorous temporal train-test split (80/20) to ensure evaluation was performed only on future, unseen data.
- **Hyperparameter Tuning**: Optimal parameters were selected using GridSearchCV combined with TimeSeriesSplit cross-validation.

## Evaluation & Results

We employed a dual evaluation strategy: Regression Metrics (for overall prediction quality) and Rare Event Metrics (for early warning utility). A significant outage was defined as any event exceeding the 95th percentile of the customers_out distribution.

|  Metric   | Value |                                      Interpretation                                      |
| :-------: | :---: | :--------------------------------------------------------------------------------------: |
|  Recall   | 0.04  |     Only identified 4% of actual significant outages (Missed 96% of severe events).      |
| Precision | 0.77  | When it predicted a major outage, it was correct 77% of the time (Low False Alarm Rate). |
| F1-score  | 0.08  |    The low score confirms the overall weakness in forecasting these events reliably.     |

The LightGBM model, despite having the best performance, presented a classic trade-off: High Precision with Catastrophic Recall. This result highlights the significant challenge in predicting the few massive spikes that dominate the time series.

## Conclusion

Achieving 5th place in this complex competition highlights the value of our robust data preprocessing and advanced feature engineering. While the gradient boosting models struggled with the fundamental challenge of forcing rare, high-magnitude spikes out of a log-transformed regression framework, our approach effectively captured the critical signals from the heterogeneous data sources (lagged hurricane events, temporal trends, and event interactions). The performance gap—a near-zero $\text{R}^2$ combined with very low Recall—provides a critical academic insight: Standard regression models are fundamentally insufficient for reliable, high-recall rare-event forecasting in this domain.

To build a utility-grade system, future work must pivot to specialized techniques:

1. Two-Stage Modeling: Implementing a sequential model: first, a highly-tuned binary classifier (optimized for Recall) to predict the occurrence of a significant event, followed by a regression model to predict the severity.
2. Spatio-Temporal Architectures: Utilizing advanced models like Spatio-Temporal Graph Neural Networks (STGCNs) to inherently model the spatial component of cascading power failures, which simple state-level features cannot capture.
