---
layout: default
title: Artorius
---

<div id="articles">
  <h1>Articles</h1>
  <ul class="posts noList">
    {% for post in site.posts %}
      <li>
        <span class="date">{{ post.date | date_to_string }}</span>
      	<h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
      </li>
    {% endfor %}
  </ul>
</div>
