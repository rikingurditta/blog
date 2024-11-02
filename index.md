---
layout: default
title: "Blog"
colour: "#CCFFFF"
---

## Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ site.baseurl }}{{ post.url }}"><strong>{{ post.date | date: "%Y-%m-%d" }}</strong>&#9;{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>