---
layout: default
---

<section class="main-content">
{% for post in site.tags.soft %}
	<article class="module color-3">
	<h2><a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a></h2>
	<span class="category">{{ post.categories }}</span>
	<p>{{ post.excerpt | strip_html | truncate: 500 }}</p>
	</article>
{% endfor %}
</section>

