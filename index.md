---
layout: default
front: true
---
{% include JB/setup %}

<section class="posts">
  {% for post in site.posts %}
    <article>
      <header>
        <h2><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h2>
        <p class="text-muted">Posted on <time datetime="post.date">{{ post.date | date_to_string }}</time></p>
      </header>
      {{ post.content }}
    </article>
  {% endfor %}
</section>
