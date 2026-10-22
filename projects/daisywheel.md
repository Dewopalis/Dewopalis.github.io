---
layout: page
title: Daisywheel REAmade
permalink: /projects/daisywheel/
---

Daisywheel REAmade is an open-source, controller-first on-screen keyboard for
Windows and Linux. It is designed for handheld PCs and living-room computers,
where a controller is often the input device already in your hands.

<figure class="project-demo">
  <img src="{{ '/assets/images/daisywheel-hello-world.gif' | relative_url }}" alt="Typing Hello World into KWrite with Daisywheel REAmade using an Xbox controller" loading="lazy">
  <figcaption>Typing into KWrite with a controller on KDE Plasma.</figcaption>
</figure>

## Status

Daisywheel is actively developed. Recent work has focused on making the full
typing and editing workflow usable from a controller, improving settings
navigation, and distributing builds for desktop platforms.

- [Download the latest release](https://github.com/Dewopalis/daisywheel-reamade/releases/latest)
- [Source code on GitHub](https://github.com/Dewopalis/daisywheel-reamade)
- [Compatibility reports](https://github.com/Dewopalis/daisywheel-reamade/issues?q=label%3Acompatibility-report)

## From the workbench
{: #writing }

{% assign daisywheel_posts = site.posts | where_exp: "post", "post.tags contains 'daisywheel'" %}
<ul class="related-list">
{% for post in daisywheel_posts limit:8 %}
  <li><span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span><br><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></li>
{% endfor %}
</ul>
