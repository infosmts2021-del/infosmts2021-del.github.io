---
layout: page
title: Blog
permalink: /blog/
---

# Travel Blog

Read our latest travel guides, tips, and stories.

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <span class="post-date">{{ post.date | date: "%b %d, %Y" }}</span>
    </li>
  {% endfor %}
</ul>
