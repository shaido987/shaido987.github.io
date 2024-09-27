---
layout: page
title: Spatial Dynamic Wind Power Forecasting Challenge
description: KDD Cup 2022 (6th place)
img: assets/img/baidukddcup2022.png
importance: 2
category: competitions
github: https://github.com/shaido987/KDD_wind_power_forecast
---

## Challenge details

The [competition](https://baidukddcup2022.github.io/)'s goal was to estimate the wind power supply of a wind farm at different time scales. The variability of wind power presents substantial challenges in incorporating it into an energy grid system. Wind power forecasting has been widely recognized as one of the most critical issues in wind power integration and operation.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/kdd_cup_wind/wind_park.jpg" title="wind farm" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            A wind park illustration.
        </div>
    </div>
    <div class="col-sm-5 mt-2 mt-md-0">
        {% include figure.liquid path="assets/img/kdd_cup_wind/turbine.gif" title="wind turbine" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Components of a wind turbine.
        </div>
    </div>
</div>

Two interesting features are different from standard wind power forecasting scenarios:

- Spatial distribution: the relative location of all wind turbines is given to model the spatial correlation among wind turbines.
- Dynamic context: important weather situations and turbine internal contexts monitored by each wind turbine are provided.

The dataset includes:

| **Column 1** | **Column Name** | **Specification**                                                        |
| :----------: | --------------- | ------------------------------------------------------------------------ |
|      1       | TurbID          | Wind turbine ID                                                          |
|      2       | Day             | Day of the record                                                        |
|      3       | Tmstamp         | Created time of the record                                               |
|      4       | Wspd (m/s)      | The wind speed recorded by the anemometer                                |
|      5       | Wdir (°)        | The angle between the wind direction and the position of turbine nacelle |
|      6       | Etmp (℃)        | Temperature of the surrounding environment                               |
|      7       | Itmp (℃)        | Temperature inside the turbine nacelle                                   |
|      8       | Ndir (°)        | Nacelle direction, i.e., the yaw angle of the nacelle                    |
|      9       | Pab1 (°)        | Pitch angle of blade 1                                                   |
|      10      | Pab2 (°)        | Pitch angle of blade 2                                                   |
|      11      | Pab3 (°)        | Pitch angle of blade 3                                                   |
|      12      | Prtv (kW)       | Reactive power                                                           |
|      13      | Patv (kW)       | Active power (target variable)                                           |

## Proposed solution

The solution we ended up proposing is detailed here {% cite kalander2022wind %} with a short presentation available on [youtube](https://www.youtube.com/watch?v=6fPL44g5h-c). In short, it's a fusion of two different models:

- Modified DLinear (MDLinear): An altered version of DLinear {% cite  Zeng2022AreTE --file references %}.
- Extreme Temporal Gated Network (XTGN): based on stacking gated temporal convolutional networks (TCNs) {% cite  dauphin2017 lea2016tcn --file references %} and nearest neighbor information diffusion.

Both models use a masked loss function that ignores missing, unknown, or abnormal values.

### Preprocessing & Feature Engineering

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/shap_no_shift.png" title="Immediate forecast" class="img-fluid rounded z-depth-1" %}
		<div class="caption">
			Immediate forecast
		</div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/shap_shift_144.png" title="One-day future forecast" class="img-fluid rounded z-depth-1" %}
		<div class="caption">
			One-day future forecast
		</div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/shap_shift_288.png" title="Two-day future forecast" class="img-fluid rounded z-depth-1" %}
		<div class="caption">
			Two-day future forecast
		</div>
    </div>
</div>

{% comment %}
Using the profile class here to left align the image without changing the sass
{% endcomment %}
<div class="profile float-right">
	{% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/heatmap.png" title="Feature correlation heatmap" class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		Feature correlation heatmap.
	</div>
</div>

From the SHAP values illustrated above, we can determine that not all features are important.

- We directly remove the directional features _Wdir_ and _Ndir_ (wind direction and nacelle yaw).
- We also remove all the temperature-related features: _Etmp_ and _Itmp_ (temperatures for the environment and inside the turbine).

{% include scripts/mathjax.liquid %}

Moreover, looking at the feature correlation heatmap, we can immediatly see that the pitch angles (_Pab1_, _Pab2_, _Pab3_) are perfectly correlated. We merge these to $$ Pab_{max}=max(Pab1, Pab2, Pab3) $$.

<h2>References</h2>
<div class="publications">
    {% bibliography --file references --cited %}
</div>
