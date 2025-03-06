---
layout: page
title: Competition Organizer
description: Co-organized a causal structure learning competition at the NeurIPS 2023 conference.
img: assets/img/csl_competition.png
importance: 2
category: causality
---

Co-organized a [causal structure learning competition](https://gcastle-hub.github.io/csl-competition/) at the [NeurIPS 2023](https://nips.cc/virtual/2023/competition/66582) conference.

The goal of this competition was to solve a causal structure learning problem in AIOps (Artificial Intelligence for IT Operations). In telecommunication networks, anomalies are commonly identified through alarms. The network operators might be facing millions of alarms per day due to the large scale and the interrelated structure of the network, as a single fault in the network can trigger a flood of various types of alarms on multiple connected devices. The goal of the operators is to quickly localize the failure point to facilitate a fast repair and recovery. However, handling all these alarms is exhausting and can quickly overwhelm the operators; hence, it must be done intelligently. There has been increasing interest in tackling the above root cause analysis (RCA) problem from a causal perspective, i.e., learning a causal graph that represents alarm relations and then using decision-making techniques (such as causal effect estimation and counterfactual inference) to efficiently identify the root cause alarm when a fault occurs. A typical RCA solution for the telecommunication network can be seen in the figure.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/rca_solution.png" title="RCA solution" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    An RCA solution in a telecommunication network.
</div>

The goal of the participants was, given a series of datasets, that for each dataset use the historical alarm data, device topology, and prior knowledge (if available) to learn a causal graph for the involved alarm types. Each learned causal graph is represented by a binary adjacency matrix and the ground truth graphs are manually labeled by experts or, for synthetic datasets, pre-set causal assumptions.
