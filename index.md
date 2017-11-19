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