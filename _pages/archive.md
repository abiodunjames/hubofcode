---
title: All Articles - Complete Blog Archive
permalink: "/archive/"
layout: page
description: Complete archive of all articles on Hubofcode covering software engineering,
  engineering leadership, DevOps, serverless architecture, and machine learning.
---

# Blog Archive

A complete list of all articles published on Hubofcode, sorted by date.

<div class="archive-list">
{% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
{% for year in posts_by_year %}
  <h2 id="{{ year.name }}">{{ year.name }}</h2>
  <ul>
    {% for post in year.items %}
    <li>
      <span class="post-date">{{ post.date | date: "%b %d" }}</span>
      <a href="{{ post.url }}">{{ post.title }}</a>
      {% if post.categories.size > 0 %}
        <span class="post-category">{{ post.categories | first }}</span>
      {% endif %}
    </li>
    {% endfor %}
  </ul>
{% endfor %}
</div>

---

## Browse by Topic

- [Software Engineering](/categories/#softwareengineering)
- [DevOps](/categories/#devops)
- [Machine Learning](/categories/#machine-learning)
- [Leadership](/categories/#leadership)

## Subscribe

Stay updated with new articles:
- [RSS Feed](/feed.xml)
- [Newsletter](/newsletter)
