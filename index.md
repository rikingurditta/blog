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
---

### Recommended reading

Here are some extremely short pieces by other people that I like:

- [*Does one have to be a genius to do maths?* by Terrence Tao](https://terrytao.wordpress.com/career-advice/does-one-have-to-be-a-genius-to-do-maths) - on talent - highly recommend this to any of my students
- [*On Practice* by Mao Zedong](https://www.marxists.org/reference/archive/mao/selected-works/volume-1/mswv1_16.htm) - a philosophical piece on dialectical materialism and the importance of actually doing stuff
- [*Humanshoe Theory* by Vincent Bevins](https://substack.com/home/post/p-151928550) - in some sense dual to [*On Authority* by Friedrich Engels](https://www.marxists.org/archive/marx/works/1872/10/authority.htm)
- [*What's that got to do with the price of eggs?* by Alexandra Scaggs](https://theehedge.substack.com/p/whats-that-got-to-do-with-the-price) - looking closely at "pricing dynamics that arise from inelastic demand"
- [*how to misgender yourself for fun* by Sam Bodrojan](https://cchelmetgirl.substack.com/p/how-to-misgender-yourself) - a collection of little things on being trans
- [*30 Rock* by Matthew Goldin](https://matthewgoldin.substack.com/p/30-rock)
