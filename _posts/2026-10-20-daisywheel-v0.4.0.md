---
layout: post
title: "Daisywheel REAmade v0.4.0: giving the wheel a little voice"
date: 2026-10-20 21:26:26 +0200
categories: [project-updates]
tags: [daisywheel]
excerpt: "Configurable shortcuts and quiet synthesized sound cues made the controller experience easier to tailor and more responsive."
---

Controller input benefits from clear feedback. A button press should feel
registered, but extra sound can quickly become distracting if it is too loud.
In v0.4.0, I worked on making those details configurable instead of assuming
one setup would suit everyone.

## Shortcuts, settings, and sound

The show/hide shortcut became configurable, and living-room mouse mode gained
an option to invert scrolling. I also brought the keyboard controls and
settings together in the main window, which made the overall flow easier to
understand.

The most noticeable addition was optional synthesized audio: MIDI-pitched cues
for moving between petals and pressing buttons. Settings let people turn the
sounds on or off, adjust volume, and preview the cues. After listening in
practice, I reduced the default output so the sounds would sit in the background
as a small extra signal rather than dominate the interaction. Previously saved
quiet levels were adjusted to preserve their perceived volume.

## Feedback should stay optional

This release reinforced a useful design lesson for me: sensory feedback is
personal. Haptics and sound can make a controller interaction clearer, but the
right amount varies with the device and environment. Giving people control over
whether those cues play, and how strongly they come through, is part of making
the keyboard comfortable to use.

### Release

[Daisywheel REAmade v0.4.0](https://github.com/Dewopalis/daisywheel-reamade/releases/tag/v0.4.0)
