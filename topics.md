---
layout: page
title: Topics
permalink: /topics/
---

Browse every post by topic.

{% assign tag_list = site.tags | sort %}
{% if tag_list.size == 0 %}
No topics yet.
{% else %}
<p class="ef-tags">
{% for tag in tag_list %}<a class="ef-tag" href="#{{ tag[0] | slugify }}">{{ tag[0] }} ({{ tag[1].size }})</a>{% endfor %}
</p>

{% for tag in tag_list %}
<h2 id="{{ tag[0] | slugify }}">{{ tag[0] }}</h2>
<ul>
{% for post in tag[1] %}
<li><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a> <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span></li>
{% endfor %}
</ul>
{% endfor %}
{% endif %}
