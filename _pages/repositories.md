---
layout: page
permalink: /repositories/
title: Code
description:
nav: true
nav_order: 4
---

---

My open-sourced code is available on [GitHub](https://github.com/shaido987). Below, I highlight a selection of recent project repositories.

{% if site.data.repositories.github_users %}

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-sm-center align-items-center">
  {% for user in site.data.repositories.github_users %}
    {% include repository/repo_user.liquid username=user %}
  {% endfor %}
</div>

## Stack Overflow

---

In addition to sharing open-source code on GitHub, I previously contributed actively to [Stack Overflow](https://stackoverflow.com/users/7579547/shaido), answering questions in areas of my expertise, primarily Apache Spark, Scala, and Python. I valued it as a way to give back to the technical community that had long supported my own work. However, I'm not very active anymore, changes introduced by the company together with the rise of AI-assisted tools, have made me reduce my participation. I still occasionally help out here and there but nothing compared to before.

<div class="row px-md-1 justify-content-sm-center">
  <a href="https://stackoverflow.com/users/7579547/shaido"><img src="https://stackoverflow.com/users/flair/7579547.png" width="278" height="77" alt="Profile for Shaido at Stack Overflow" title="profile for Shaido at Stack Overflow"></a>
  <a href="https://stackexchange.com/users/10271255"><img src="https://stackexchange.com/users/flair/10271255.png" width="278" height="77" alt="Profile for Shaido on Stack Exchange" title="profile for Shaido on Stack Exchange"></a>  
</div>

## GitHub Repositories

---

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for repo in site.data.repositories.github_repos %}
    {% include repository/repo.liquid repository=repo %}
  {% endfor %}
</div>
{% endif %}
