---
layout: page
title: Metabolism
permalink: /metabolism/
---

{% for post in site.categories.Metabolism %}
<span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a>
{% endfor %}