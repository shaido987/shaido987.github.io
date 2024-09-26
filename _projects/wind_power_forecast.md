---
layout: page
title: Spatial Dynamic Wind Power Forecasting Challenge
description: KDD Cup 2022 (6th place)
img: assets/img/baidukddcup2022.png
importance: 2
category: competitions
related_publications: true
github: https://github.com/shaido987/KDD_wind_power_forecast
---

# Challenge details

The competition's goal was to estimate the wind power supply of a wind farm at different time scales. The variability of wind power presents substantial challenges in incorporating it into an 
energy grid system. Wind power forecasting has been widely recognized as one of the most critical issues in wind power integration and operation. 

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/kdd_cup_wind/wind_park.jpg" title="wind farm" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/kdd_cup_wind/turbine.gif" title="wind turbine" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
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

# Proposed solution

{% cite kalander2022wind %}
