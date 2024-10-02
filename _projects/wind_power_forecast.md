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

- Modified DLinear (MDLinear): An altered version of DLinear {% cite Zeng2022AreTE --file references %}.
- Extreme Temporal Gated Network (XTGN): based on stacking gated temporal convolutional networks (TCNs) {% cite dauphin2017 lea2016tcn --file references %} and nearest neighbor information diffusion.

Both models use a masked loss function that ignores missing, unknown, or abnormal values.

## Preprocessing & Feature Engineering

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
Using the profile class here to left align the image without changing the sass.
{% endcomment %}

<div class="profile float-right">
	{% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/heatmap.png" title="Feature correlation heatmap" class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		Feature correlation heatmap.
	</div>
</div>

From the SHAP values illustrated above, we can determine that not all features are important.

- We directly remove the directional features $$ Wdir $$ and $$ Ndir $$ (wind direction and nacelle yaw).
- We also remove all the temperature-related features: $$ Etmp $$ and $$ Itmp $$ (temperatures for the environment and inside the turbine).

Moreover, looking at the feature correlation heatmap, we can immediately see that the pitch angles ($$ Pab1 $$, $$ Pab2 $$, $$ Pab3 $$) are perfectly correlated. We merge these to

$$
Pab_{max}=max(Pab1, Pab2, Pab3).
$$

This gives us a final feature set of five features: turbine ID, wind speed, maximum pitch angle, reactive power, and active power (also the target variable).

### MDLinear: Modified DLinear

The method is based on DLinear {% cite Zeng2022AreTE --file references %} which is a simple but effective method that has been shown to outperform numerous transformer-based models on multiple time series forecasting tasks. By design, the method operates on univariate time series data. For multivariate time series, the forecasts of each feature are thus independent of the others. The method first decomposes the time series into trend and residual components. Two one-layer linear networks ($$ W_t $$ and $$ W_r $$) are then applied to the respective component before the two results are merged into a final forecast output.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/mdlinear.png" title="Overview of the MDLinear method." class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		Overview of the MDLinear method.
	</div>
    </div>
</div>

We modify DLinear to exploit all available information by adding an additional linear layer at the end to consolidate the information from all input features and denote our method MDLinear. The complete method design is illustrated in the above figure.

<div class="profile float-right">
	{% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/clusters_with_id2.png" title="Wind turbine cluster and ID assignments" class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		Wind turbine cluster and ID assignments.
	</div>
</div>

We apply some additional feature engineering steps for MDLinear. The _TurbID_ is split into two separate features: $$ cluster $$ and $$ ID $$ by adhering to the wind turbines’ x-coordinates, while the $$ ID $$ is assigned
based on the y-coordinates. Moreover, a new feature is added, $$ cluster\_avg $$, with the average $$ Patv $$ of each cluster. For cluster $$ c $$ with a set of wind turbines $$ C $$, we have

$$
cluster\_avg_{c} = \frac{1}{n_c} \sum_{i=1}^{n_c} Patv^{C(i)},
$$

where $$ n_c $$ is the number of wind turbines in cluster $$ c $$ and $$ Patv^{C(i)} $$ the wind power of the $$ i $$-th wind turbine in $$ C $$. Note that $$ cluster\_avg $$ has the same value for all wind turbines within the same cluster at the same timestep.

<div class="profile float-right">
	{% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/multi_forecast.png" title="MDLinear train and forecast strategy" class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		MDLinear train and forecast strategy.
	</div>
</div>

Training a single model to make future forecasts is not always advantageous. Short-term and long-term patterns can vastly differ and a model that focuses on the full horizon may fail to capture important short-term patterns. Therefore, in MDLinear, we train four separate models with increasing forecast horizons [72, 188, 216, 288] and merge these to create a final prediction. The last predicted 72 timesteps of each model are used. The figure to the right illustrates the procedure.

### XTGN: eXtreme Temporal Gated Network

The second part of our proposed solution is XGTN, inspired by the idea of converting a forecasting problem into a pattern-matching task. The dynamical chaotic grid system produces complicated features with sudden jumps and uncertain wind power generation with strong randomness, making it difficult to generate accurate forecasts. To quickly respond to varying wind power patterns and abrupt changes caused by external reasons such as wind turbine renovation or active power controlling, we apply a Temporal Convolutional Network (TCN) {% cite lea2016tcn --file references %} based framework to acquire the necessary information on existing representative patterns and then generalize it for forecasting.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/GTCN_Framework.png" title="The framework of XTGN." class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		The framework of XTGN.
	</div>
    </div>
</div>

In XTGN, the inputs are first transformed by the gated temporal convolution module (Gated TCN, detailed in the left part of the above figure) followed by a 2D convolutional layer and a linear layer. An information diffusion mechanism (shown in the dashed box) is performed only during inference to get a reliable wind power prediction. Specifically, a neighborhood aggregation corrects the values obtained from the last linear layer to develop a more accurate prediction. Considering the observation that nearby wind turbines exhibit similar wind power patterns, it is natural to describe the underlying graph structure of the system using a distance matrix. Here, we generate a $$ k $$ nearest neighbor ($$ k $$-NN) graph based on the cosine similarity between the geographic location of each wind turbine pair.

Considering this, the final output for wind turbine $$ x $$ with forecast $$ Y $$ will be

$$
\alpha \cdot Y + \frac{1−\alpha}{n} \sum_{v \in N(x)} v.
$$

### Fused model and results

<div class="profile float-right">
	{% include figure.liquid loading="eager" path="assets/img/publication_preview/wind.png" title="A prediction for a single wind turbine" class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		A prediction for a single wind turbine at time t.
	</div>
</div>

We consolidate the predictions from MDLinear and XTGN into a fused forecast. We denote the predictions for a single wind turbine at timestep $$ t $$ as $$ Y^m \in \mathcal{R}^{288x1}$$ and $$ Y^t \in \mathcal{R}^{288x1}$$ for MDLinear and XTGN, respectively. We empirically found that a simple averaging of the two forecasts at each timestep achieves robust results, see the ablation in the below table. The fused forecast for each wind turbine is thus

$$
Y = \frac{Y^m + Y^t}{2}.
$$

The process is illustrated in the figure to the right, which depicts a typical 288-length prediction for a single timestep of a single wind turbine.

The final results of our method and various baselines and ablation with various fusion strategies can be seen in the tables below. **Our results ended up placing us 6th out of 2500 or so teams.**

<div class="row">
    <div class="col-sm-5 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/scores.png" title="Offline scores and inference times for the methods" class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		Offline scores and inference times for the methods.
	</div>
    </div>
    <div class="col-sm-5 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/kdd_cup_wind/ablation.png" title="Different fusion strategies" class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		Different fusion strategies. For the time splits, the method in parentheses is used for the first timesteps.
	</div>
    </div>
</div>

<h2>References</h2>
<div class="publications">
    {% bibliography --file references --cited %}
</div>
