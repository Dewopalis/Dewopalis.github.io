---
layout: post
title: "Daisywheel REAmade v0.5.0: making the whole workflow controller-first"
date: 2026-10-20 22:00:01 +0200
categories: [projects, daisywheel, releases]
excerpt: "Controller-only settings and text editing brought the project closer to a living-room setup that does not depend on a mouse."
---

If the goal is to use Daisywheel REAmade from the couch, needing a physical
mouse to change its settings breaks the experience. And when a typo slips in,
reaching for a keyboard just to move the cursor can be just as disruptive. In
v0.5.0, I focused on closing both gaps.

## Settings from the controller

Holding the configured shortcut now opens Settings, where the D-pad, sticks,
face buttons, and shoulder buttons can navigate and change options. The wheel
shows progress while the shortcut is held and gives a confirmation cue when the
action is ready. New installations also start with the Living room / desktop
profile, while existing saved preferences are kept.

## Editing without putting the controller down

The new editing mode lets the left stick move the text cursor while RT is held.
LB selects text, RB moves by words, and holding both allows word-by-word
selection. A configurable shared stick deadzone—from 5% to 45%—helps account for
different controllers and stick drift while using the wheel, pointer, or editing
controls.

I also tightened the settings layout so its controls fit narrow windows, and
moved several UI widgets out of the large main source file. The release passed
Linux build and test checks, alongside tagged Windows and Linux package builds.
Real applications, target handhelds, and external permission prompts still
deserve testing beyond what automated checks can cover.

For me, this release was a step toward making the full experience controller-
first—not just typing, but also setup and correction. There is still more to
learn from testing across devices and desktop environments.

### Release

[Daisywheel REAmade v0.5.0](https://github.com/Dewopalis/daisywheel-reamade/releases/tag/v0.5.0)
