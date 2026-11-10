---
layout: post
title: "Open Golden Sun: starting, stopping, then restarting the rebuild"
date: 2026-11-10 19:32:00 +0200
categories: [workbench]
tags: [open-golden-sun, godot, reverse-engineering, agents]
excerpt: "Three weeks into rebuilding one of my favourite games, I nearly stopped. I'm keeping it going for myself and learning how to work with coding agents along the way."
---

I played a ton of Golden Sun on my Game Boy SP when I was younger. I still have a physical copy. It was one of my favourite games back then, and it's still an absolute blast to play.

That's why I wanted to try rebuilding it. Open Golden Sun is my attempt to bring *Golden Sun: The Broken Seal* and *The Lost Age* together as one moddable game. I'm using Godot for now. The planned Continuum system would carry progress between the campaigns, including support for the original passwords. Multiple platforms and six languages are also on the wish list.

As I write this, the project is only about three weeks into development. I've already managed to get to the point of nearly stopping and then starting again.

## Why I nearly stopped

The progress reports sounded encouraging, with dialogue, battle calculations and saving all getting worked on. But the demos and tests didn't always go through the main game scene. There were still gaps between those systems, so I couldn't play through the things that sounded finished.

Then there was [Golden Sun Reunion](https://www.reddit.com/r/GoldenSun/comments/1xll1in/golden_sun_reunion_im_rebuilding_golden_sun_the/). I came across it on Reddit: another project combining the two games in Godot. It is extremely similar to what I had in mind, and from what I've seen, it looks much further along. I've joined their Discord and am actively following the development there.

I'm excited to see where Reunion goes. But seeing someone else so far along with almost the same idea did make me wonder whether to keep going with mine.

## Why I came back to it

I still want to build this for myself. There's a lot I want to learn, and lately the biggest part of that has been figuring out a different way of working with coding agents.

For the restart, I went back to the main scene: moving around, interacting with things, entering a basic battle and keeping the rewards afterward. Then closing the game completely and checking whether I could load it and carry on.

My brother is working on a Rugby08 PS2 rebuild, and a screenshot of his agent workflow gave me an idea for organizing mine. One coordinates the work, others take on specific jobs, and leads help out when an area needs more coordination.

Some jobs involve figuring out data formats. Others involve getting that data into the game or writing tools to test it. Each worker gets a defined task and returns its changes and test results. The coordinator puts the changes together, and a separate agent reviews them without inheriting the author's whole conversation.

<figure class="project-demo">
  <a href="{{ '/assets/images/open-golden-sun-agent-workflow.svg' | relative_url }}">
    <img src="{{ '/assets/images/open-golden-sun-agent-workflow.svg' | relative_url }}" alt="Workflow showing a coordinator, three on-demand work domains, bounded workers and an independent reviewer, with a maximum of four active agents including the coordinator" width="1440" height="1060" loading="lazy">
  </a>
  <figcaption>The workflow as of November 10. These are roles, not a count of agents running at once. Open the diagram for a larger view.</figcaption>
</figure>

I can have up to four agents active in this setup, including the coordinator. They don't all need to be busy all the time. When several are changing code, they get separate Git worktrees so they aren't writing over each other's files. One agent then combines the changes and tests them together. The reviewer can use the same model as the author, but starts with fresh context.

I'm also trying to keep the cost sensible. Cheaper models can handle small, well-defined jobs, but the savings disappear if a wrong interpretation of a format needs several rounds of fixes. Writing long instructions and repeatedly reviewing the same work takes time too.

## The parser was happy. But there was nothing shiny to look at.

One map-reading attempt started eight entries off in a resource table. The parser still finished. It had found bytes it could read, but the camera bounds and offsets coming out of them made no sense. Checking the table against known reference points exposed the wrong starting position.

In another case, an agent reversed two byte planes and wrote tests that expected the same reversed order. So the tests passed with the bug still there. It only came out when the code was checked against the earlier specification and reference source.

I've been referring to [Alchemy](https://github.com/PascalPixel/alchemy), a decompilation project, to understand how the original data and code work. I keep track of the exact version being read, then use those findings to write the reconstruction's own code. I also use [mGBA](https://mgba.io/) locally to observe the original games.

I've also been looking at [GSRecomp](https://github.com/Shmargus/GSRecomp), which uses static recompilation to make a native port. I'm actively playing through the game with it while working on this rebuild. I'd be lying if I said I was playing for research, though. I just want to replay the games, in all honesty.

Its launcher, progress transfer and automation are useful references for the rebuild. I considered building on it, but I'm continuing with Godot for now. Open Golden Sun isn't a fork of GSRecomp.

## Saving includes what happens next

Along with the player's location and inventory, a save needs to remember story flags and progression. It also needs the random-number state. Without that, saving halfway through a battle and restarting could change what happens next.

Each save also records which content pack it belongs to. Even changing a texture changes the pack's identity, so a save won't quietly load different content just because the area names happen to match.

The test content now runs through the main scene and can continue after closing and reopening the game. Basic physical battles carry HP, coins and experience back into the field. There's still a lot of combat left to implement.

The visual tests read pixels from the OpenGL viewport to check three static layers, how they overlap and whether the camera moves correctly. I also had the checks run with deliberately wrong pixel and camera expectations. Those failed, as they should. This round of visual work passed 48 Godot cases, 37 Python reference checks and 26 rendering, replay and rejection checks.

## Getting a real map through it

The first local map trial has now passed. Three 480 by 912 pixel layers went through conversion and into the main scene. The run handled extraction, import and six OpenGL replay processes without anyone needing to navigate with a controller.

Two rounds compared an uninterrupted run with saving, closing the game and loading it in a fresh process. The session state, random-number state and content identity carried through, and the repeated runs matched.

Across 18 captured frames, 449,280 background pixel comparisons matched the produced layers byte for byte, excluding fixed areas covering the UI and placeholder actors. Those checks compare the renderer's output with the converted layers. I still need to check those layers against the original game's pixels.

There is more to do before I can play through an original region. The layers are static, and the original animation, collision, events and NPC behavior still need work. Full combat, language and platform support are also unfinished.

The [project repository](https://github.com/Dewopalis/Open-Golden-Sun) is public. Original ROMs, art, dialogue, converted content and captures stay local. The repository contains the reconstruction code, tools and authored test content.

For now, I have a map going through the main scene and a lot left to figure out.
