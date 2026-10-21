---
layout: default
title: "Dewald's Workbench"
---

<div class="home-page">
  <section class="home-hero">
    <p class="eyebrow">Projects · experiments · side quests</p>
    <h1>Dewald's Workbench</h1>
    <p class="home-tagline">Building things, breaking things, and occasionally figuring out how they work.</p>
    <p>Software experiments, retro games, Linux tinkering, and whatever rabbit hole comes next.</p>
  </section>

  <section class="home-section" aria-labelledby="start-here">
    <h2 id="start-here">Start here</h2>
    <ul class="post-list start-list">
      {% for post in site.posts %}
        {% if post.url contains '/starting-this-blog/' or post.url contains '/how-daisywheel-reamade-began/' %}
          <li>
            <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
            <h3><a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></h3>
            <p class="post-excerpt">{{ post.excerpt | strip_html | strip }}</p>
          </li>
        {% endif %}
      {% endfor %}
    </ul>
  </section>

  <section class="home-section" aria-labelledby="latest-posts">
    <h2 id="latest-posts">Latest posts</h2>
    <ul class="post-list">
      {% for post in site.posts %}
        {% unless post.url contains '/starting-this-blog/' or post.url contains '/how-daisywheel-reamade-began/' %}
          <li>
            <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
            <h3><a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></h3>
            <p class="post-excerpt">{{ post.excerpt | strip_html | strip }}</p>
          </li>
        {% endunless %}
      {% endfor %}
    </ul>
  </section>
</div>
