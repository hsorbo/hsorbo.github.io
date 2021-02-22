---
layout: page
title: Håvard Sørbø
#tagline: Supporting tagline
---
## About
Developer, lives in Bergen, Norway

## iClue
[iClue](https://github.com/hsorbo/iclue)

## Latest Posts
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

