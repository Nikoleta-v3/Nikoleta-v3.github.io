---
layout: page
permalink: /blog/
title: Blog posts
---


<div id="archives">
  <section id="archive">
      {%for post in site.posts %}
          <p><b><a href="{{ site.baseurl }}{{ post.url }}">{% if post.title and post.title != "" %}{{post.title}}{% else %}{{post.excerpt |strip_html}}{%endif%}</a></b> {% if post.date and post.date != "" %}{{ post.date | date: "%e %B %Y" }}{%endif%}.</p>
          {% endfor %}
  </section>
</div>
