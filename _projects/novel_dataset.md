---
layout: page
title: Novel Dataset
description: Dataset with translated novels
img: assets/img/graph.png
importance: 1
category: fun
github: https://github.com/shaido987/novel-dataset
github_stars: true
---

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
	{% include figure.liquid loading="eager" path="assets/img/graph.png" title="Graph" class="img-fluid rounded z-depth-1" %}
	<div class="caption">
		Graph with various novels. Node color from novel category and edges from recommended novels. The graph is created by randomly selecting a node and propagating to a max depth of 3.
	</div>
    </div>
</div>

This is a minor project which I started to obtain some interesting graph data. It creates a dataset from [novelupdates](https://www.novelupdates.com) containing information about translated novels. The dataset contains translated English novels from eight original languages (Chinese, Japanese, Korean, Malaysian, Filipino, Indonesian, Khmer, and Thai) with 21,831 novels in total after the latest update. Both individual novel statistics such as the number of chapters and ranking as well as relations to other novels are available. These various relations are what makes the dataset especially interesting as they can be used to form a graph.

Both the code and the dataset can be found on [github](https://github.com/shaido987/novel-dataset). Normally, I update the dataset once a year or so to include any added novels and updates to all the novel information.
