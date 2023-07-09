---
layout: page
title: On Ketosis
permalink: /ketosis/
order: 1
---

{% for post in site.categories.Ketosis %}
<span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a>
{% endfor %}