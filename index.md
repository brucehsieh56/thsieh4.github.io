---
layout: default
---

# Hello!

Welcome to my website. I'm Bruce. I'd like to share my works and interesting experience in Machine Learning and Data Science, please have a look at my [posts](/posts/post_home/).

# Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.date | date: "%B %d, %Y" }}: {{ post.title }}</a>
    </li>
  {% endfor %}
</ul>


# Posts

{% capture tag %}
  {% for tag in site.tags %}
    {{ tag[0] }}
  {% endfor %}
{% endcapture %}
{% assign sortedtags = tag | split:' ' | sort %}

{% for tag in sortedtags %}
  <h3 id="{{ tag }}">{{ tag }}</h3>
  <ul>
  {% for post in site.tags[tag] %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
  </ul>
{% endfor %}