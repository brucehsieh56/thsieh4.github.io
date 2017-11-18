# Hello!

Welcome to my website. I'm Bruce. I'd like to share my works and interesting experience in Machine Learning and Data Science, please have a look at my [posts](/posts/post_home/).


<p>
    {% for tag in site.tags %}
    <a href="/tags/{{ tag[0] }}/" 
    style="font-size: {{ tag[1] | size | times: 2 | plus: 10 }}px">
        {{ tag[0] }} | 
    </a>
    {% endfor %}
</p>
