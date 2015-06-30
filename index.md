---
layout: default
front: true
---
{% include JB/setup %}

<section class="posts">
  {% for post in site.posts %}
    <article>
      <header class="row">
        <h2 class="col-md-9 col-md-offset-3"><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h2>
      </header>
      <div class="row">
    		<aside class="meta col-md-3">
		      <p class="text-muted"><time datetime="{{ post.date }}">{{ post.date | date_to_string }}</time></p>
          {% unless post.categories == empty %}
            <ul class="tag_box inline">
              <li><i class="icon-folder-open"></i></li>
              {% assign categories_list = post.categories %}
              {% include JB/categories_list %}
            </ul>
          {% endunless %}

          {% unless post.tags == empty %}
            <ul class="tag_box inline">
              <li><i class="icon-tags"></i></li>
              {% assign tags_list = post.tags %}
              {% include JB/tags_list %}
            </ul>
          {% endunless %}
        </aside>
        <div class="col-md-9">
          {{ post.excerpt }}
          {% unless post.excerpt == post.content %}
            <p>
              <a href="{{ BASE_PATH }}{{ post.url }}">Read more...</a>
            </p>
          {% endunless %}
        </article>
      </div>
    </article>
  {% endfor %}
</section>
