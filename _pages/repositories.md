---
layout: page
permalink: /repositories/
title: Code
description:
nav: true
nav_order: 4
---

---

My open-sourced code can be found on [GitHub](https://github.com/shaido987). Below are a couple of recent project repositories.

{% if site.data.repositories.github_users %}

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-sm-center align-items-center">
  {% for user in site.data.repositories.github_users %}
    {% include repository/repo_user.liquid username=user %}
  {% endfor %}
</div>

## Stack Overflow

---

In addition to open-source code on GitHub, I've spent some time on [Stack Overflow](https://stackoverflow.com/users/7579547/shaido) answering questions in subjects I'm familiar with (mainly Apache-spark, Scala, and Python-related subjects). I see it as a way of giving back to the larger community, as Stack Overflow is an indispensable website in my day-to-day work. I also participate on other Stack Exchange sites to a lesser degree.

<div class="container">
  <div class="row row-cols-md-auto px-md-1 justify-content-center">
      <a href="https://stackoverflow.com/users/7579547/shaido"><img src="https://stackoverflow.com/users/flair/7579547.png" width="278" height="77" alt="profile for Shaido at Stack Overflow" title="profile for Shaido at Stack Overflow"></a>
    <a href="https://stackexchange.com/users/10271255"><img src="https://stackexchange.com/users/flair/10271255.png" width="278" height="77" alt="profile for Shaido on Stack Exchange" title="profile for Shaido on Stack Exchange"></a>
  </div>
</div>

## GitHub Repositories

---

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for repo in site.data.repositories.github_repos %}
    {% include repository/repo.liquid repository=repo %}
  {% endfor %}
</div>
{% endif %}
