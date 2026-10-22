---
layout: post
title: "Why automating a Daisywheel demo was harder than it looked"
date: 2026-10-22 16:00:00 +0200
categories: [workbench]
tags: [daisywheel]
featured: true
excerpt: "A repeatable demo sounded simple. Then I had to deal with real focus, KDE's virtual keyboard, Wayland capture permissions, and two monitors."
---

I wanted a simple way to keep Daisywheel's demos up to date. Ideally, one
command would open a text field, show the keyboard, type something, move the
window, adjust its transparency, and save a clean recording. That would be
more repeatable than recording my desktop by hand every time.

The first challenge was deciding what the demo should prove. Daisywheel can
show its wheel and play back controller input inside its own test panel. That
is useful for checking the wheel, but it does not prove the part I care about
most: the keyboard appearing in response to a real text field and entering text
into another application.

I did get a scripted typing demo working with a virtual controller and the
same controller-event path the app normally uses. It types into Daisywheel's
local test panel, so it is a useful repeatable check of the wheel interaction.
It is not the same as showing KDE opening the keyboard in Firefox or typing
into a separate application. Keeping that distinction clear matters. A demo
should show what it claims to show.

## The desktop is part of the feature

For the focus demo, I wanted to use a private Firefox window and the real KDE
virtual-keyboard setting. The intended sequence was simple: focus a text field,
click away so the keyboard closes, then focus the field again. The operating
system should handle the keyboard popup, just as it would during normal use.

That made the environment part of the test. The browser needed to be on the
right display, the pointer automation had to reach the browser rather than an
underlying window, and KDE needed to treat the field as an active text input.
Making the clicks happen was not enough. I also needed evidence that the
keyboard had actually opened for the right reason.

Then screen capture got involved. My desktop has a 2560×1080 monitor and a
1920×1080 monitor. KDE's capture portal exposed them as one 4480×1080 desktop
stream, rather than giving the recorder a separate stream for each display.
The recording command's idea of “the screen” also did not match the monitor I
intended to show. One attempt captured the wrong display, including an
unrelated video that happened to be open there. I stopped, removed that failed
capture and its generated files, and left the older blog recordings alone.

<figure class="project-demo">
  <img src="{{ '/assets/images/wayland-capture-layout.svg' | relative_url }}" alt="Diagram showing the 2560 by 1080 and 1920 by 1080 monitors combined into one 4480 by 1080 capture stream, with the demo display highlighted" loading="lazy">
  <figcaption>A simplified view of the combined desktop stream that confused the recording attempt.</figcaption>
</figure>

On Wayland, screen capture and automated input also pass through desktop
permissions. That is a good boundary for privacy and security, but it means a
recording script has to work with the compositor's choices instead of assuming
that every window and monitor is freely available to it. A tool that silently
records more than the intended screen is not a tool I want to publish.

## What I am taking away

There are really two different jobs here. An app-level scripted demo can
reliably exercise Daisywheel's controller and wheel logic. A live desktop demo
has to coordinate the application, text focus, KDE's virtual keyboard, input
permissions, window placement, monitor selection, and recording. The second
one is closer to an integration test of the whole desktop than a simple video
export.

I stopped the live-recording attempt rather than keep adding workarounds before
I could trust the result. The existing recordings still show the real
experience, and the small scripted typing demo is useful for repeatable checks.
If I return to automatic desktop recordings, the first requirement will be a
capture path that can prove which display it is recording before it starts.

For now, the failed attempt was still useful. It showed me where the boundary
is between testing Daisywheel itself and testing how a desktop environment
integrates it. Those are both worth demonstrating, but they need different
tools and different claims.

So I am still stuck manually recording and cutting demo videos.
