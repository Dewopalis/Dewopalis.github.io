---
layout: post
title: "Daisywheel REAmade v0.1.0: from prototype to a real desktop keyboard"
date: 2026-10-14 16:19:10 +0200
categories: [project-updates]
tags: [daisywheel]
excerpt: "The first release made Daisywheel REAmade usable beyond its test panel, with Windows text input, automatic activation, and a path toward downloadable packages."
---

With v0.1.0, I could use Daisywheel REAmade to type into other Windows apps.
Until then, I had been typing into its own test panel.

## Getting text into other apps

The early prototype was useful for checking whether the wheel felt right, but
typing into a test panel wasn't enough. The point was to use a controller to
enter text wherever a text field had focus. In v0.1.0, I added Windows system
input and automatic keyboard activation, so the keyboard could join the normal
text-entry flow.

I also fixed the pinned Windows overlay position so it stayed where I put it
when reopened.

## Making it available to other people

GitHub Actions started building Windows, Debian, Fedora, and AppImage packages, and the project
got a GPL-3.0 license, contribution guidance, and clearer package instructions.
The first package workflow needed a few fixes along the way.

### Release

[Daisywheel REAmade v0.1.0](https://github.com/Dewopalis/daisywheel-reamade/releases/tag/v0.1.0)
