---
layout: page
title: Håvard Sørbø
#tagline: Supporting tagline
---
{% include JB/setup %}
## About
Developer, lives in Bergen, Norway

## iClue
[iClue](/iclue.html)

## Latest Posts
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

