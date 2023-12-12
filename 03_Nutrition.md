---
layout: page
permalink: /Nutrition/
title: On Nutrition
order: 3
---

<figure align = "center">
  <p align = "center">
    <img src = "/casein-micelle-and-fat-globule-in-milk.jpeg" alt="micelle-and-fat-globule" style="height: 300px; width: 300px;"/>
  </p>
  <figcaption><i>This illustration depicts the cross section of a fat globule (yellow, upper left) and a casein micelle (tan, lower center) collected from cow's milk. These 2 classes of molecules, fat and proteins, are foundational to our body's daily operations and have been a source of great discomfort for many. Credit given to David S. Goodsell, RCSB Protein Data Bank.</i>
  </figcaption>
</figure>

## Dedication and Introduction
This page is dedicated to every adipocyte, every myocyte, every neuron, and the rest of their gang for causing me waves of distress and fleeting moments of championship. You guys were the real educators.

Below is a series of posts dedicated towards unraveling the mores of nutrition. I've done my best to avoid injecting any un-warned bias concerning the topic at hand. The information therefore should be founded in studies and largely affirmed to be true. If not, I will predicate the information confirming that the following is conjecture.

## Protein Series

Undoubtedly, most cursorily understand that proteins play an important role in our daily lives and that we need to eat some defined amount on a regular basis. But the knowledge concerning protein and our diet both starts and ends there. An abundance of questions still remain. How much protein do we eat? What exactly is happening when we eat protein? Does the protein source we eat matter?

This series is dedicated to unraveling the nutritional value of protein.

{% for post in site.categories.Nutrition %}
<span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a>
{% endfor %}