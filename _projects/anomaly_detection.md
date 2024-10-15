---
layout: page
title: Timeseries Anomaly Detection
description: KDD Cup 2021 (2nd place)
img: assets/img/anomaly_detection/kdd_cup_anomaly.png
importance: 1
category: competitions
---

## Challenge details

The [competition](https://compete.hexagon-ml.com/practice/competition/39/) considers numerous, highly different, time series from various sources and the task was to detect anomalies in these. Univariate time series detection is often highly dataset-dependent and what algorithms will work for a particular dataset is non-obvious. The competition and accompanying dataset were released to encourage academia and industry to work towards a more general solution that applies to various anomaly detection scenarios.

- 250 different time series datasets with highly varied characteristics.
- Each time series is partitioned into train and test data. The train data is guaranteed to not contain any anomalies while the test data has exactly 1 anomaly.
- Each time series has a single dimension (univariate).
- No labels are given for the duration of the competition.

## Proposed solution

We applied a set of algorithms, each specialized on a specific type of anomaly (discrete, spike, discord, variance, etc.), in a fixed order until a confident prediction was made. Those algorithms that focused on easier anomaly types and those that were more conservative were used first. **Our solution placed 2nd out of 624 teams.** A youtube video is available for a short solution overview: https://www.youtube.com/watch?v=4PdlUcmwWu0

<div class="profile float-right">
	{% include figure.liquid loading="eager" path="assets/img/anomaly_detection/solution.png" title="Solution overview" class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		Mixture-of-experts solution overview.
	</div>
</div>

For our multi-method solution to work, there are a few key components:

- Measurement of confidence levels for method selection.
- Period inference: all the time series in the data are periodical (although it can be irregular and there can be multiple patterns). For simplicity we base most model parameters on an inferred period. In this way, two time series that have the same shape but different sampling rates are treated the same.
- Ensemble learning: the period inference is not perfect and there is no guarantee that using the model parameters based on the period is the best possible selection. In some of the base methods, we therefore try a set of periods based on the inferred period $$ p : \{f \cdot p | f \in \matcal{F}\} $$, where $$ \mathcal{F} $$ is a set of multipliers. For example: Time series $$ T $$ has an inferred period $$ 𝑝=128 $$ and method $$ A $$ considers the set of multipliers $$ \mathcal{F}=\{0.5, 1.0, 2.0\} $$. We then run method $$ A $$ with periods $$ \{64, 128, 256\} $$ and selects the result with the highest confidence score.

### Confidence level

<div class="profile float-right">
	{% include figure.liquid loading="eager" path="assets/img/anomaly_detection/confidence_level.png" title="Confidence level" class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		Example of anomaly scores of a single expert model.
	</div>
</div>

We compute the confidence level by considering the difference between the maximum anomaly score and the second-largest score. The anomaly scores in the same peak are excluded (e.g., 9.612 is not considered). The difference is then compared with an acceptance threshold which can differ for each algorithm. If the difference is above the threshold then the confidence is above 1 and the prediction is accepted.

Illustration from the example in the figure:

Difference: $$ \frac{max⁡\(anomaly scores\)}{(sec_max⁡\(anomaly scores\)} − 1 = \frac{\(12.338−5.091\)}{5.091} = 1.423 $$  
Confidence (with a threshold of $$ 0.3 $$): $$ \frac{𝑑𝑖𝑓𝑓𝑒𝑟𝑒𝑛𝑐𝑒}{𝑡ℎ𝑟𝑒𝑠ℎ𝑜𝑙𝑑} = \frac{1.423}{0.3} = 4.745 $$

The expert is deemed confident as $$ 4.745 > 1 $$ and the anomaly prediction is this used.

### Experts

<div class="row">
    <div class="col-sm mt-5 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/shap_no_shift.png" title="Base algorithms" class="img-fluid rounded z-depth-1" %}
		<div class="caption">
			Base algorithms to be used as experts.
		</div>
</div>

