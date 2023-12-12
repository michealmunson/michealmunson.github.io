---
layout: page
permalink: /Data/
title: On Data
order: 2
---

## Introduction

Making conclusions given data is an important skill and something I find fun. Below are a series of posts exploring data analysis.

## Posts

# On Math

{% for post in site.categories.Data %}
<span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a>
{% endfor %}