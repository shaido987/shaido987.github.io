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

In addition to sharing open-source code on GitHub, I actively contribute to [Stack Overflow](https://stackoverflow.com/users/7579547/shaido) by answering questions in areas of my expertise, primarily related to Apache Spark, Scala, and Python. I view this as a meaningful way to give back to the broader technical community, especially since Stack Overflow is an invaluable resource in my daily work. I also engage, to a lesser extent, on other Stack Exchange sites.

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
