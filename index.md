---
layout: default
title: "Blog"
colour: "#FFDDFF"
---

## Posts

<style>
li::marker {
color: var(--marker, currentColor);
}
</style>
<ul>
  {% for post in site.posts %}
    <li style="--marker: {{ post.colour }}">
      <a href="{{ site.baseurl }}{{ post.url }}"><strong>{{ post.date | date: "%Y-%m-%d" }}</strong>&#9;{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>