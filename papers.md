---
layout: page
title: Papers/Notes
permalink: /papers/
---

<ul class="papers-list">
  {% for paper in site.data.papers %}
  <li class="paper-item">
    <div class="paper-title"><a href="{{ paper.url }}">{{ paper.title }}</a></div>
    <div class="paper-meta">{% if paper.date %}{{ paper.date }} · {% endif %}{{ paper.description }}</div>
  </li>
  {% endfor %}
</ul>