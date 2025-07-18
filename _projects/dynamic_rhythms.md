---
layout: page
title: Dynamic Rhythms
description: Time series prediction challenge for power outages (5nd place)
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

To tackle this problem, we first have to have a good understanding of the data. To make a long story short, a couple of key insights were found:

- Target variable skewness: The target variable (customers without power) was highly skewed with occasional large spikes, confirming the rare but impactful nature of significant outage events.
- Missing values: Missing values in event count columns, particularly for rare events (some with >95% missing values), were imputed with 0.
- Strongest correlations: The tropical storm and hurricane events had the most pronounced positive correlation with the target variable, highlighting their severe impact.

