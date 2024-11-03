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

<link rel="prefetch" href="https://www.shutterstock.com/editorial/image-editorial/MaTbMe0cO9DcEez1NzMwODY=/eddie-izzard-1500w-13365630h.jpg"/>
<link rel="prefetch" href="https://64.media.tumblr.com/11a185e7ee421707c2fc134e8980d7a1/tumblr_piz1d0zpf01tlq10co1_1280.jpg"/>
<link rel="prefetch" href="https://upload.wikimedia.org/wikipedia/en/b/bc/Jeffery_young_thug.jpg"/>
<link rel="prefetch" href="https://rikingurditta.github.io/blog/img/lees-dress.jpg"/>
<link rel="prefetch" href="https://rikingurditta.github.io/blog/img/ortho-project.jpg"/>
<link rel="prefetch" href="https://rikingurditta.github.io/blog/img/encampment-aftermath.jpg"/>
