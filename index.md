---
layout: default
---
<style>
body { overflow-x: hidden; }
.page-content { padding-top: 0 !important; }
.ef-hero { position: relative; overflow: hidden; background: #1e2a5a; color: #f4efe3; width: 100vw; margin-left: calc(50% - 50vw); padding: 5rem 1.5rem 7rem; }
.ef-hero::after { content: ""; position: absolute; width: 900px; height: 900px; border-radius: 50%; background: #e9dfc9; right: -200px; bottom: -760px; }
.ef-hero-inner { position: relative; z-index: 1; max-width: 760px; margin: 0 auto; }
.ef-hero h1 { font-family: "Iowan Old Style", Palatino, Georgia, serif; font-size: clamp(2.6rem, 9vw, 5rem); line-height: 1; letter-spacing: -0.03em; font-weight: 700; margin: 0 0 1rem; color: #f4efe3; }
.ef-hero p { font-size: 1.2rem; max-width: 30rem; color: #c9cdea; margin: 0; }
.ef-label { font-size: 1rem; color: #6b6f85; margin: 2.5rem 0 0; }
.ef-post { padding: 1.8rem 0; border-bottom: 1px solid #e6e2d8; }
.ef-meta { font-size: 0.85rem; color: #1e2a5a; }
.ef-meta span + span { margin-left: 1rem; color: #6b6f85; }
.ef-post h2 { font-family: "Iowan Old Style", Palatino, Georgia, serif; font-size: 1.9rem; line-height: 1.2; font-weight: 600; margin: 0.3rem 0 0.5rem; }
.ef-post h2 a { color: #1a1c2c; text-decoration: none; }
.ef-post h2 a:hover { color: #1e2a5a; text-decoration: underline; }
.ef-post p { color: #555a70; margin: 0; }
</style>
<section class="ef-hero">
<div class="ef-hero-inner">
<h1>Earth and Faith</h1>
<p>Questions are never dangerous. A personal blog about religion, science and reason.</p>
</div>
</section>
<p class="ef-label">Latest posts</p>
{% for post in site.posts %}
<article class="ef-post">
<div class="ef-meta"><span>{{ post.date | date: "%b %-d, %Y" }}</span><span>{{ post.content | number_of_words | divided_by: 200 | plus: 1 }} min read</span></div>
<h2><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></h2>
<p>{{ post.excerpt | strip_html | strip_newlines | truncatewords: 38 }}</p>
</article>
{% endfor %}
