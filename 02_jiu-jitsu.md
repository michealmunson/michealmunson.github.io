---
layout: page
title: Jiu Jitsu
permalink: /jiu-jitsu/
order: 2
---

{% for post in site.categories.JiuJitsu %}
<span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a>
{% endfor %}