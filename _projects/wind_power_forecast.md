---
layout: page
title: Wind Power Forecasting
description: Participated in the KDD Cup 2022 Spatial Dynamic Wind Power Forecasting Challenge (6th place)
img: assets/img/kdd_cup_wind/kdd_cup_wind_start_page.png
importance: 2
category: competitions
github: https://github.com/shaido987/KDD_wind_power_forecast
github_stars: true
---

## Challenge details

The [KDD Cup 2022 competition](https://baidukddcup2022.github.io/) aimed to predict the wind power output of a wind farm across various time horizons. Accurate wind power forecasting is crucial for integrating wind energy into grid systems, given the inherent variability of wind. Wind power forecasting has been widely recognized as one of the most critical issues in wind power integration and operation.

Key details of the competition include:
- The wind farm comprises 134 wind turbines.
- Each turbine provides a time series with 10-minute intervals and 10 features.
- The task requires predicting wind power from 0 to 48 hours into the future (288 steps) at each timestep.
- The spatial locations of the wind turbines are provided.
- Approximately 2,500 teams participated in the competition.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/shap_no_shift.png" title="Immediate forecast" class="img-fluid rounded z-depth-1" %}
		<div class="caption">
			Immediate forecast.
		</div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/shap_shift_144.png" title="One-day future forecast" class="img-fluid rounded z-depth-1" %}
		<div class="caption">
			One-day future forecast.
		</div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/shap_shift_288.png" title="Two-day future forecast" class="img-fluid rounded z-depth-1" %}
		<div class="caption">
			Two-day future forecast.
		</div>
    </div>
</div>
<div class="caption">
	Feature importance across different forecast horizons, visualized using SHAP values.
</div>

## Proposed solution
<div class="profile float-right">
	{% include figure.liquid loading="eager" path="assets/img/publication_preview/wind.png" title="A prediction for a single wind turbine" class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		A prediction for a single wind turbine at time t.
	</div>
</div>

Our proposed solution, detailed in {% cite kalander2022wind %}, combines two innovative models. A brief overview is also available in a [YouTube presentation]((https://www.youtube.com/watch?v=6fPL44g5h-c)). The approach integrates:

- **Modified DLinear (MDLinear):** An enhanced adaptation of the DLinear model {% cite Zeng2022AreTE --file references %}.
- **Extreme Temporal Gated Network (XTGN):** A novel architecture built by stacking gated temporal convolutional networks (TCNs) {% cite dauphin2017 lea2016tcn --file references %} and incorporating nearest-neighbor information diffusion.

Both models employ a masked loss function to handle missing, unknown, or anomalous values effectively. **Our solution achieved a remarkable 6th place out of approximately 2,500 competing teams.**

<h2>References</h2>
<div class="publications">
    {% bibliography --file references --cited %}
</div>
