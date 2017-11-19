---
layout: default
---

# Hello!

Welcome to my website. I'm Bruce. I'd like to share my works and interesting experience in Machine Learning and Data Science, please have a look at my [posts](/posts/post_home/).


<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.date | date: "%B %d, %Y" }}{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>


{% capture tags %}
  {% for tag in site.tags %}
    {{ tag[0] }}
  {% endfor %}
{% endcapture %}
{% assign sortedtags = tags | split:' ' | sort %}
{% for tag in sortedtags %}
    <a href="/tags/{{ tag }}/">{{ tag }}</a><br>
{% endfor %}
