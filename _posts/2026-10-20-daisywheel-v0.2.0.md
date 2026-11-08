---
layout: post
title: "Daisywheel REAmade v0.2.0: making installation less of a project"
date: 2026-10-20 17:16:05 +0200
categories: [project-updates]
tags: [daisywheel]
excerpt: "A guided AppImage installer made the first Linux setup friendlier, while downloadable packages brought the project closer to everyday use."
---

Once there were packages to download, I ran into the next usability problem:
installing the Linux version still asked people to understand several manual
steps. A release should be something a curious person can try, not a small
systems-administration exercise.

## A guided Linux setup

In v0.2.0, the Linux AppImage gained a first-run installer. It offered an
**Install for this user** flow, put the app in a stable location, and registered
it with KDE Plasma so it could be selected under **System Settings → Keyboard
→ Virtual Keyboard**. It deliberately didn't switch the current keyboard
automatically; the user stayed in control of that system setting.

The release also provided a Windows setup installer and portable ZIP, Debian
package, Fedora package, and Linux AppImage, with checksums.

### Release

[Daisywheel REAmade v0.2.0](https://github.com/Dewopalis/daisywheel-reamade/releases/tag/v0.2.0)
