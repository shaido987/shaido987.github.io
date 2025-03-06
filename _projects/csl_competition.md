---
layout: page
title: Competition Organizer
description: Co-organized a causal structure learning competition at the NeurIPS 2023 conference.
img: assets/img/csl_competition.png
importance: 2
category: causality
---

Co-organized a [causal structure learning competition](https://gcastle-hub.github.io/csl-competition/) at the [NeurIPS 2023](https://nips.cc/virtual/2023/competition/66582) conference.

The goal of this competition was to address a causal structure learning problem in AIOps (Artificial Intelligence for IT Operations), with a focus on root cause analysis (RCA) in telecommunication networks. In such networks, anomalies are often detected through alarms. Due to the large-scale and interconnected nature of these networks, operators may face millions of alarms daily, as a single fault can trigger a cascade of diverse alarm types across multiple connected devices. The operators’ objective is to swiftly pinpoint the failure source to enable rapid repair and recovery. However, manually managing this volume of alarms is overwhelming, necessitating intelligent and efficient solutions.

There has been growing interest in approaching the RCA problem from a causal perspective—specifically, by learning a causal graph that models alarm relationships and applying decision-making techniques (such as causal effect estimation and counterfactual inference) to identify the root cause of a fault. A typical RCA solution for telecommunication networks is illustrated in the figure below.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/rca_solution.png" title="RCA solution" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    An RCA solution in a telecommunication network.
</div>

The goal of the participants was, given a series of datasets, that for each dataset use the historical alarm data, device topology, and prior knowledge (if available) to learn a causal graph for the involved alarm types. Each learned causal graph is represented by a binary adjacency matrix and the ground truth graphs are manually labeled by experts or, for synthetic datasets, pre-set causal assumptions.

Participants in the competition were tasked with using historical alarm data, device topology, and prior knowledge (when available) to learn a causal graph for the alarm types in each provided dataset. Each learned causal graph was represented as a binary adjacency matrix, with ground truth graphs either manually labeled by domain experts or, in the case of synthetic datasets, derived from predefined causal assumptions.

To conclude the competition, we organized a [virtual workshop at NeurIPS 2023](https://nips.cc/virtual/2023/competition/66582), titled "Causal Structure Learning from Event Sequences and Prior Knowledge". This workshop, co-organized with a team of experts, provided a platform to showcase the competition outcomes. The event featured presentations by the top six winning teams 
