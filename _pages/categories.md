---
title: Browse Articles by Category
permalink: "/categories/"
layout: page
description: Browse all articles on Hubofcode organized by category including Software
  Engineering, DevOps, Machine Learning, and Engineering Leadership.
---

# Categories

Browse articles by topic. Click on a category to see all related posts.

<div id="archives">
{% for category in site.categories %}
  <div class="archive-group">
    {% capture category_name %}{{ category | first }}{% endcapture %}
    <div id="{{ category_name | slugify }}"></div>
    
    <h2 class="category-head">{{ category_name }} <span class="post-count">({{ category | last | size }})</span></h2>
    <a name="{{ category_name | slugify }}"></a>
    
    <ul class="category-posts">
    {% for post in site.categories[category_name] %}
      <li class="archive-item">
        <span class="post-date">{{ post.date | date: "%b %Y" }}</span>
        <a href="{{ site.baseurl }}{{ post.url }}">{% if post.title and post.title != "" %}{{ post.title }}{% else %}{{ post.excerpt | strip_html }}{% endif %}</a>
      </li>
    {% endfor %}
    </ul>
  </div>
{% endfor %}
</div>
