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
    		<div class="col-md-3">
		      <p class="text-muted">Posted on <time datetime="{{ post.date }}">{{ post.date | date_to_string }}</time></p>
          {% unless page.categories == empty %}
            <ul class="tag_box inline">
              <li><i class="icon-folder-open"></i></li>
              {% assign categories_list = page.categories %}
              {% include JB/categories_list %}
            </ul>
          {% endunless %}

          {% unless page.tags == empty %}
            <ul class="tag_box inline">
              <li><i class="icon-tags"></i></li>
              {% assign tags_list = page.tags %}
              {% include JB/tags_list %}
            </ul>
          {% endunless %}
        </div>
        <div class="col-md-9">
          {{ post.content }}
        </article>
      </div>
    </article>
  {% endfor %}
</section>
