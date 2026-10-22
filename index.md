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
    <p class="start-reading">New here? <a href="{{ '/2026/10/21/starting-this-blog/' | relative_url }}">Why I started writing</a> · <a href="{{ '/2026/10/21/how-daisywheel-reamade-began/' | relative_url }}">How Daisywheel began</a></p>
  </section>

  <section class="home-section" aria-labelledby="featured-workbench">
    <h2 id="featured-workbench">Featured from the workbench</h2>
    <div class="feature-grid">
      {% assign shown_featured = 0 %}
      {% for post in site.posts %}
        {% if post.featured and shown_featured < 2 %}
          <article class="feature-card">
            <p class="post-kind-label">From the Workbench</p>
            <h3><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></h3>
            <p>{{ post.excerpt | strip_html | strip }}</p>
            <a class="read-link" href="{{ post.url | relative_url }}">Read the story <span aria-hidden="true">→</span></a>
          </article>
          {% assign shown_featured = shown_featured | plus: 1 %}
        {% endif %}
      {% endfor %}
    </div>
  </section>

  <section class="home-section" aria-labelledby="on-the-workbench">
    <h2 id="on-the-workbench">On the workbench</h2>
    <div class="project-grid">
      <article class="project-card">
        <p class="project-status">Active project</p>
        <h3><a href="{{ '/projects/daisywheel/' | relative_url }}">Daisywheel REAmade</a></h3>
        <p>Controller-first desktop keyboard for handheld PCs and living-room computers.</p>
        <a class="read-link" href="{{ '/projects/daisywheel/' | relative_url }}">Explore the project <span aria-hidden="true">→</span></a>
      </article>
      <article class="project-card">
        <p class="project-status">Early experiment</p>
        <h3><a href="{{ '/projects/#open-golden-sun' | relative_url }}">Open Golden Sun</a></h3>
        <p>A Godot rebuild experiment exploring one connected Golden Sun campaign with mod support.</p>
        <a class="read-link" href="{{ '/projects/#open-golden-sun' | relative_url }}">Project notes <span aria-hidden="true">→</span></a>
      </article>
    </div>
  </section>

  <section class="home-section" aria-labelledby="latest-updates">
    <h2 id="latest-updates">Latest project updates</h2>
    <ul class="update-list">
      {% assign shown_updates = 0 %}
      {% for post in site.posts %}
        {% if post.categories contains 'project-updates' and shown_updates < 3 %}
          <li>
            <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
            <h3><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></h3>
            <p>{{ post.excerpt | strip_html | strip }}</p>
          </li>
          {% assign shown_updates = shown_updates | plus: 1 %}
        {% endif %}
      {% endfor %}
    </ul>
    <p class="all-updates"><a href="{{ '/projects/daisywheel/' | relative_url }}#writing">All Daisywheel writing and updates <span aria-hidden="true">→</span></a></p>
  </section>
</div>
