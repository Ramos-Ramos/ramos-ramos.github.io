---
layout: page
title: sports graphics
permalink: /sports_graphics/
description: Amateur sports graphics made for fun.
nav: false
horizontal: false
---

Sometimes we make sports graphics as a hobby. We've also made some things for [Yakyu Cosmopolitan](https://twitter.com/yakyucosmo) and [KBO in English](https://twitter.com/KBO_ENG) (If you're interested in keeping up with NPB or KBO, check them out!).

<!-- pages/projects.md -->
<div class="sports_graphics">

<!-- Generate cards for each project -->
{% assign sorted_sports_graphics = site.sports_graphics | sort: "date" | reverse %}
{% if page.horizontal %}
<div class="container">
  <div class="row row-cols-1 row-cols-md-2">
  {% for sports_graphic in sorted_sports_graphics %}
    {% include sports_graphics_horizontal.liquid %}
  {% endfor %}
  </div>
</div>
{% else %}
<div class="row row-cols-1 row-cols-md-3">
  {% for sports_graphic in sorted_sports_graphics %}
    {% include sports_graphics.liquid %}
  {% endfor %}
</div>
{% endif %}
</div>
