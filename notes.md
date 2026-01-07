---
layout: page
title: Notes
permalink: /notes/
---

<ul class="papers-list">
  {% for note in site.data.notes %}
  <li class="paper-item">
    <div class="paper-title"><a href="{{ note.url }}">{{ note.title }}</a></div>
    {% if note.comments %}<div class="paper-comments">{{ note.comments | strip_newlines }}</div>{% endif %}
  </li>
  {% endfor %}
</ul>
