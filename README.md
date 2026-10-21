# Dewald's Field Notes

A small Jekyll blog for notes on learning, building, and figuring things out.
It is designed to publish on GitHub Pages from this repository.

## Writing a post

Create a Markdown file in `_posts/` named `YYYY-MM-DD-short-title.md`. Add
front matter at the top:

```yaml
---
layout: post
title: "A useful title"
date: 2026-10-21 12:00:00 +0200
categories: [project]
---
```

Write the post below the second `---`. Push it to `main` and GitHub Actions will
build and publish the site.

## Publishing setup

1. Create a public GitHub repository named `Dewopalis.github.io` and push this
   project to its `main` branch.
2. In the repository, open **Settings → Pages** and select **GitHub Actions** as
   the build and deployment source.
3. The workflow in `.github/workflows/pages.yml` will publish the site at
   `https://dewopalis.github.io`.

The first deployment can take a few minutes. A custom domain is optional and
would need to be purchased separately.
