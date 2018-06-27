---
layout: article
title: "Projects"
date: 2016-06-27
modified: 2016-06-27
excerpt: "My Projects"
image:
  feature:
  teaser:
  thumb:
share: false
ads: false
---

<div class="tiles">
{% for post in site.categories.projects %}
  {% include post-grid.html %}
{% endfor %}
</div><!-- /.tiles -->
