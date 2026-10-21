---
layout: post
title: "Daisywheel REAmade v0.3.0: bringing mouse controls to the couch"
date: 2026-10-20 18:34:16 +0200
categories: [project-updates, daisywheel]
excerpt: "Controller profiles, mouse input, repeat deletion, and haptics helped turn the wheel into a more capable living-room input tool."
---

Typing from the couch often means more than entering a few letters. You may
need to move a pointer, click a control, scroll a page, or correct a mistake.
For v0.3.0, I started building those parts of the living-room experience into
Daisywheel REAmade.

## A controller can do more

The release introduced Handheld and Living room / desktop profiles, plus
adjustable pointer speed and optional controller mouse mode. While the keyboard
is open, holding LT and using the left stick moves the pointer; R1 and R2 click,
and X/Y and the D-pad scroll. Keeping mouse mode available only while the
keyboard is visible was important: I didn't want its controls interfering with
a game when the keyboard was closed.

I also added configurable haptic feedback and hold-to-repeat Backspace. These
small interactions address two common frustrations: not knowing a selection
registered, and having to press repeatedly to erase a longer mistake. The wheel
layout and button indicators received some refinement too, including more
useful resizing behavior.

## Testing the real setup

The release pipeline checked Windows, DEB/AppImage, and RPM packages, including
installation and startup smoke tests. Those checks helped catch packaging
problems, but they couldn't replace trying the app with an actual controller
and target applications. Steam Deck Gaming Mode, touchscreen activation, and a
wider range of devices still needed testing.

v0.3.0 made the project feel less like a keyboard demo and more like an input
tool for a couch setup. It also made clear how much controller behavior depends
on the hardware, desktop, and application around it.

### Release

[Daisywheel REAmade v0.3.0](https://github.com/Dewopalis/daisywheel-reamade/releases/tag/v0.3.0)
