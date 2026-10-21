---
layout: post
title: "How Daisywheel REAmade began: REA, old ideas, and a first prototype"
date: 2026-10-21 12:00:00 +0200
categories: [workbench, daisywheel]
excerpt: "The first decisions behind a controller-first keyboard for Windows and Linux, and the projects that helped me find a direction."
---

The idea for Daisywheel REAmade started with a simple question: could I bring
Steam's old daisywheel-style keyboard to the computers I actually wanted to use
with a controller?

I had handheld PCs such as the Steam Deck and ROG Ally in mind, but I didn't
want the project to stop there. A controller-first keyboard could also make
typing from the couch on a living-room PC much less awkward. I wanted it to work
on Windows and Linux, and to behave like a real on-screen keyboard: appear when
I needed to enter text, then get out of the way when I was done.

## Looking at what already existed

Steam's Big Picture daisywheel was the starting point for the interaction: move
a stick to choose a group of letters, then use a face button to pick a
character. It is a compact way to type with a controller, where a full keyboard
layout can be slow to navigate.

Two projects helped me think about how that idea could be used outside Steam.
[DaisywheelJS](https://github.com/likethemammal/daisywheeljs) brings the Steam
wheel to JavaScript and CSS and reads controllers through the browser Gamepad
API. Its stated aim was to give browser games a better option than showing a
QWERTY keyboard to someone holding a controller. [AnyWheel](https://github.com/EvilJagaGenius/AnyWheel)
takes a different route: it is a small Python and Pygame program that runs
alongside another window, reads a gamepad, and sends text to the selected
window.

Those examples showed me that the wheel could be useful in more than one
setting. They also highlighted the gap I wanted to explore: a native desktop
keyboard that could work across applications and connect to the operating
system's own text-entry and virtual-keyboard behavior.

## Using REA to explore the reference

I brought REA into the research so I could inspect the existing implementation
instead of relying only on screenshots and memory. I asked it to analyze the
JavaScript project, focusing on the entry point, wheel and petal components,
and the controller and input modules. That gave me a structured way to look at
how the reference project was organized.

REA's analysis was static: it parsed the selected source files without running
the application. I selected only the files relevant to the wheel and controller
flow, so the result was partial by design. It helped me orient myself, but it
couldn't tell me how the interface felt in use or prove how it behaved in a
live browser. For that, I also read the project documentation and treated the
reference implementations as prior art, not code to transplant.

## Keeping the first version small

The first native prototype used C++17, Qt 6 Widgets, SDL2, and CMake. I kept the
basic wheel deliberately simple: eight petals, four characters per petal, the
left stick to choose a petal, and the face buttons to select a character. A
local test field gave me a safe, visible way to check controller input and
character selection independently from the experimental KDE system-keyboard
path.

That split turned out to matter. Drawing a wheel and reading a controller are
one problem; delivering text to whichever application currently has focus is
another. On Linux, the early experiment had to work with KDE's virtual-keyboard
and Wayland input-method plumbing. Windows needed its own approach. The visual
idea could be shared, but the desktop integration could not simply be assumed
to work the same way on both systems.

## What I learned at the start

The first milestone wasn't a polished keyboard. It was a small proof that the
controller interaction made sense and that I could build a native foundation
for it. Once I tried to make it appear in real applications, the project became
as much about focus, operating-system integration, packaging, and testing as it
was about the wheel itself.

That is still the part I find interesting: taking a familiar interaction and
making it fit a different kind of computer. Daisywheel REAmade has grown well
beyond that first prototype, but the original goal remains the same: make text
entry feel natural when a controller is the input device in your hands.

### References

- [DaisywheelJS](https://github.com/likethemammal/daisywheeljs)
- [AnyWheel](https://github.com/EvilJagaGenius/AnyWheel)
- [Daisywheel REAmade](https://github.com/Dewopalis/daisywheel-reamade)
