---
layout: page
title: "TechBlog"
permalink: /tech/blogs/
---

{{ page.title }}

  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}