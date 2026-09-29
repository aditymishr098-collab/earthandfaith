---
layout: default
---
<style>
body { overflow-x: hidden; }
.page-content { padding: 0 !important; }
.ef { width: 100vw; margin-left: calc(50% - 50vw); display: grid; grid-template-columns: 2fr 3fr; }
.ef-side { position: relative; overflow: hidden; background: #12332b; color: #f3ead2; padding: 4rem 2.5rem 9rem; }
.ef-side::after { content: ""; position: absolute; left: 50%; bottom: -150px; width: 280px; height: 280px; margin-left: -140px; border-radius: 50%; background: #e8b64a; }
.ef-side-inner { position: relative; z-index: 1; max-width: 26rem; margin-left: auto; }
.ef-side h1 { font-family: "Iowan Old Style", Palatino, Georgia, serif; font-size: clamp(2.6rem, 6vw, 4.2rem); line-height: 1.02; letter-spacing: -0.03em; font-weight: 700; margin: 0 0 1.2rem; color: #f3ead2; }
.ef-side p { font-size: 1.1rem; line-height: 1.6; color: #b9cfc4; margin: 0; }
.ef-main { background: #ffffff; padding: 3rem 2.5rem; }
.ef-main-inner { max-width: 38rem; }
.ef-label { font-size: 1rem; color: #5d6b66; margin: 0 0 0.5rem; }
.ef-post { padding: 1.6rem 0; border-bottom: 1px solid #e3e8e4; }
.ef-meta { font-size: 0.85rem; color: #a06a12; }
.ef-meta span + span { margin-left: 1rem; color: #5d6b66; }
.ef-post h2 { font-family: "Iowan Old Style", Palatino, Georgia, serif; font-size: 1.75rem; line-height: 1.2; font-weight: 600; margin: 0.3rem 0 0.5rem; }
.ef-post h2 a { color: #12332b; text-decoration: none; }
.ef-post h2 a:hover { color: #a06a12; text-decoration: underline; }
.ef-post p { color: #4a5852; margin: 0; }
@media (max-width: 800px) {
  .ef { grid-template-columns: 1fr; }
  .ef-side { padding: 3rem 1.5rem 8rem; }
  .ef-side-inner { margin-left: 0; }
  .ef-main { padding: 2rem 1.5rem; }
}
</style>
<div class="ef">
<section class="ef-side">
<div class="ef-side-inner">
<h1>Earth and Faith</h1>
<p>Questions are never dangerous. A personal blog about religion, science and reason.</p>
</div>
</section>
<section class="ef-main">
<div class="ef-main-inner">
<p class="ef-label">Latest posts</p>
{% for post in site.posts %}
<article class="ef-post">
<div class="ef-meta"><span>{{ post.date | date: "%b %-d, %Y" }}</span><span>{{ post.content | number_of_words | divided_by: 200 | plus: 1 }} min read</span></div>
<h2><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></h2>
<p>{{ post.excerpt | strip_html | strip_newlines | truncatewords: 38 }}</p>
</article>
{% endfor %}
</div>
</section>
</div>
