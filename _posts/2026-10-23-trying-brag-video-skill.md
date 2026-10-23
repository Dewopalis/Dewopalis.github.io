---
layout: post
title: "Sometimes you can find some cool stuff while doomscrolling"
date: 2026-10-23 12:00:00 +0200
categories: [workbench]
tags: [tools, video, workflow]
featured: true
excerpt: "I tried the brag coding-agent skill on a project I already knew. The useful part was shaping a clear story from real footage, not recording a new demo."
---

I came across [brag](https://github.com/latent-spaces/brag) through an Instagram post. It is a skill for coding agents that uses Hyperframes to turn a project into a short video. The idea sounded fun, so I wanted to see what would happen if I tried it on something of my own.

I used Daisywheel as the test project because I already knew the code and had a few screen recordings ready. The post is really about trying the tool, though. Daisywheel was just a useful subject for the experiment.

## Starting with a story

The first question was not how to animate a title. It was what the video should show. We settled on one simple sequence: focus a text field, let the keyboard appear, type with a controller, and show the text that landed in the editor. Mostly because that is the basic demo gif I already had in the repo

That gave the video a real beginning and result. It also kept the claims grounded in things the project already does. I did not want a generic feature reel or a made-up version of the interface.

<figure class="project-demo">
  <video controls playsinline preload="metadata" poster="{{ '/assets/images/brag-experiment-poster.jpg' | relative_url }}" aria-label="A short video made with the brag skill, showing a text field opening Daisywheel and controller typing">
    <source src="{{ '/assets/videos/brag-experiment.mp4' | relative_url }}" type="video/mp4">
    Your browser does not support embedded video. <a href="{{ '/assets/videos/brag-experiment.mp4' | relative_url }}">Download the video</a>.
  </video>
  <figcaption>A 20-second promo assembled from existing Daisywheel screen recordings.</figcaption>
</figure>

Music: “Happy Beats &amp; Business Moves Vol. 9” by Sascha Ende, licensed under <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a> via <a href="https://ende.app/en/song/12874-happy-beats-business-moves-vol-9-by-ende-dot-app">ende.app</a>.

## It did not record the desktop for me

One important detail: brag arranged recordings I already had. It did not launch Daisywheel, operate the desktop, or create those original interactions. I had recorded the Firefox popup and controller typing demos earlier, then used them as source material.

That distinction mattered because I had previously tried to automate live desktop recordings and ran into focus, Wayland capture, and multiple-monitor complications. This experiment started one step later in the process. It helped turn existing footage into a short story, but it did not solve the recording problem itself. So this won't help in my pursuit of laziness when it comes to recording demos.

## A small rendering wrinkle

The first render raised a warning about the converted recordings having sparse keyframes. Hyperframes renders by seeking to exact points in time, so long gaps between keyframes can make a clip freeze or show the wrong moment. I re-encoded the recordings with a keyframe every second, then rendered again. The second render completed without that warning.

The source GIFs were only 480 by 416 pixels. The large captions are clear, but the tiny text inside the desktop recordings is still soft when enlarged. Better source footage would improve the result more than another layer of animation.

## Would I use it again?

Eh, I'll play around with it or a similar tool again. The poster image was a nice bonus. It came from the finished video, and I liked it enough to add it to my repo for some visual flair. The Daisywheel icon was already in the project, but it worked nicely on the end card. I think this skill could be useful on a more mature project though.

For now I still am trying to figure out the most simple way to automate actually showing off the tool
