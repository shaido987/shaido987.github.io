---
layout: page
title: Wind Power Forecasting
description: KDD Cup 2022 (6th place)
img: assets/img/kdd_cup_wind/kdd_cup_wind_start_page.png
importance: 2
category: competitions
github: https://github.com/shaido987/KDD_wind_power_forecast
---

## Challenge details

The [competition](https://baidukddcup2022.github.io/)'s goal was to estimate the wind power supply of a wind farm at different time scales. The variability of wind power presents substantial challenges in incorporating it into an energy grid system. Wind power forecasting has been widely recognized as one of the most critical issues in wind power integration and operation.

- The wind farm has 134 wind turbines.
- Each wind turbine has a time series with 10-minute timesteps and 10 features.
- At each timestep, the goal is to predict 0h to 48h into the future (288 steps).
- The wind turbines' location is known.
- The competition had around 2500 registered teams.

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

## Proposed solution

The solution we ended up proposing is detailed here {% cite kalander2022wind %} with a short presentation available on [youtube](https://www.youtube.com/watch?v=6fPL44g5h-c). In short, it's a fusion of two different models:

- Modified DLinear (MDLinear): An altered version of DLinear {% cite Zeng2022AreTE --file references %}.
- Extreme Temporal Gated Network (XTGN): based on stacking gated temporal convolutional networks (TCNs) {% cite dauphin2017 lea2016tcn --file references %} and nearest neighbor information diffusion.

Both models use a masked loss function that ignores missing, unknown, or abnormal values. **Our results ended up placing us 6th out of 2500 or so teams.**

<div class="profile float-right">
	{% include figure.liquid loading="eager" path="assets/img/publication_preview/wind.png" title="A prediction for a single wind turbine" class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		A prediction for a single wind turbine at time t.
	</div>
</div>

<h2>References</h2>
<div class="publications">
    {% bibliography --file references --cited %}
</div>
