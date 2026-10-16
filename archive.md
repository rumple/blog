---
layout: page
title: Archived Thoughts
permalink: /archive/
---
{% assign months = site.posts | group_by_exp: "p", "p.date | date: '%Y-%m'" %}
{% for m in months %}
<h3 id="{{ m.name }}">{{ m.items.first.date | date: "%B %Y" }}</h3>
<ul>
{% for p in m.items %}<li><a href="{{ p.url | relative_url }}">{{ p.title }}</a></li>
{% endfor %}</ul>
{% endfor %}
