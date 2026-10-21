---
layout: post
title: "Daisywheel REAmade v0.1.0: from prototype to a real desktop keyboard"
date: 2026-10-14 16:19:10 +0200
categories: [project-updates, daisywheel]
excerpt: "The first release made Daisywheel REAmade usable beyond its test panel, with Windows text input, automatic activation, and a path toward downloadable packages."
---

The first release was a turning point: Daisywheel REAmade could begin acting
like a desktop keyboard, rather than just a controller demo with a text box of
its own.

## Getting text into other apps

The early prototype was useful for checking whether the wheel felt right, but
typing into a test panel wasn't enough. The point was to use a controller to
enter text wherever a text field had focus. In v0.1.0, I added Windows system
input and automatic keyboard activation, so the keyboard could join the normal
text-entry flow.

I also fixed the pinned Windows overlay position so it stayed where I put it
when reopened. Small details like this mattered because an on-screen keyboard
needs to feel like part of the desktop, not a window that has to be continually
repaired.

## Making it available to other people

This release also laid groundwork around the application itself. GitHub Actions
started building Windows, Debian, Fedora, and AppImage packages, and the project
got a GPL-3.0 license, contribution guidance, and clearer package instructions.
The first package workflow needed fixes, which was a useful reminder that
automating a build is only the beginning: the artifacts also need to install and
start correctly.

v0.1.0 was a small first release, but it changed the question I was working on.
The wheel already worked in isolation. Now I could focus on making it fit into
real desktop environments and making it practical for someone else to try.

### Release

[Daisywheel REAmade v0.1.0](https://github.com/Dewopalis/daisywheel-reamade/releases/tag/v0.1.0)
