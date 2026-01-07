---
layout: page
title: Papers
permalink: /papers/
---

<ul class="papers-list">
  {% for paper in site.data.papers %}
  <li class="paper-item">
    <div class="paper-title"><a href="{{ paper.url }}">{{ paper.title }}</a></div>
    {% if paper.authors %}<div class="paper-meta">{{ paper.authors }}</div>{% endif %}
    {% if paper.comments %}<div class="paper-comments">{{ paper.comments | strip_newlines }}</div>{% endif %}
  </li>
  {% endfor %}
</ul>
