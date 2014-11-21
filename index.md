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
        </header>
        <details>Posted on <time>{{ post.date | date_to_string }}</time></details>
        {{ post.content }}
    </article>
  {% endfor %}
</section>
