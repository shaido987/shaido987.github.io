---
layout: page
title: Timeseries Anomaly Detection
description: KDD Cup 2021 (2th place)
img: assets/img/anomaly_detection/kdd_cup_anomaly.png
importance: 1
category: competitions
---

## Challenge details


## Proposed solution

We applied a set of algorithms, each specialized on a specific type of anomaly (discrete, spike, discord, variance, etc.), in a fixed order until a confident prediction was made. Those algorithms that focused on easier anomaly types and those that were more conservative were used first. The key components were period inference (individual algorithm parameters were mostly based on the period), confidence level computation and thresholding, and ensemble learning.

