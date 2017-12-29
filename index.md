---
layout: page
group: navigation
---


<ul>
  {% for post in site.posts %}
    <li>
      <a href="/blog{{ post.url }}">{{ post.title }}</a>
      <p>{{ post.excerpt }}</p>
    </li>
  {% endfor %}
</ul>
