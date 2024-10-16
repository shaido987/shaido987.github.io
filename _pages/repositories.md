---
layout: page
permalink: /repositories/
title: Code
description: You can find implementations for some recent projects below.
nav: true
nav_order: 4
---

{% if site.data.repositories.github_users %}

## GitHub User

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for user in site.data.repositories.github_users %}
    {% include repository/repo_user.liquid username=user %}
  {% endfor %}
</div>

## Stack Overflow

In addition to open-source code on GitHub, I've spent some time on Stack Overflow answering questions in subjects I'm familiar with (mainly Apache-spark, scala, and python-related subjects). I see it as a way of giving back to the larger community as Stack Overflow is an indispensable website in my day-to-day work.

<a href="https://stackoverflow.com/users/7579547/shaido"><img src="https://stackoverflow.com/users/flair/7579547.png" width="208" height="58" alt="profile for Shaido at Stack Overflow, Q&amp;A for professional and enthusiast programmers" title="profile for Shaido at Stack Overflow, Q&amp;A for professional and enthusiast programmers"></a>

## GitHub Repositories

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for repo in site.data.repositories.github_repos %}
    {% include repository/repo.liquid repository=repo %}
  {% endfor %}
</div>
{% endif %}
