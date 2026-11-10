---
layout: post
title: "Open Golden Sun: starting, stopping, then restarting the rebuild"
date: 2026-11-10 19:32:00 +0200
categories: [workbench]
tags: [open-golden-sun, godot, reverse-engineering, agents]
excerpt: "Finding Golden Sun Reunion nearly convinced me to stop my own rebuild. I'm keeping it going for myself, with plenty to learn about working with coding agents."
---

I've mentioned Open Golden Sun on this blog before, but haven't written much about it yet. The idea is to rebuild *Golden Sun: The Broken Seal* and *The Lost Age* as one moddable game. I'm using Godot for now, with a planned system called Continuum to carry progress between the two campaigns, including support for the original passwords. Multiple platforms and six languages are also on the wish list.

I came across [Golden Sun Reunion on Reddit](https://www.reddit.com/r/GoldenSun/comments/1xll1in/golden_sun_reunion_im_rebuilding_golden_sun_the/), another project combining the two games in Godot. It is extremely similar to what I had in mind, and from what I've seen, it looks much further along. I've joined their Discord and am actively following the development there.

Seeing it made me wonder whether I should keep going with my own version. I nearly stopped. I'm excited to see where Reunion goes, but I still want to work on mine for myself. I've been learning a lot about how to work with coding agents, and I want to keep experimenting with that.

My own project had problems too. The progress reports sounded encouraging, with dialogue, battle calculations and saving all getting worked on. But the demos and tests didn't always go through the main game scene. There were still gaps between those systems, so I couldn't play through the things that sounded finished.

I started again from that main scene: moving around, interacting with things, entering a basic battle and keeping the rewards afterward. Then closing the game completely and checking whether I could load it and carry on.

## The parser was happy. But there was nothing shiny to look at.

One map-reading attempt started eight entries off in a resource table. The parser still finished. It had found bytes it could read, but the camera bounds and offsets coming out of them made no sense. Checking the table against known reference points exposed the wrong starting position.

In another case, an agent reversed two byte planes and wrote tests that expected the same reversed order. So the tests passed with the bug still there. It only came out when the code was checked against the earlier specification and reference source.

I've been referring to [Alchemy](https://github.com/PascalPixel/alchemy), a decompilation project, to understand how the original data and code work. I keep track of the exact version being read, then use those findings to write the reconstruction's own code. I also use [mGBA](https://mgba.io/) locally to observe the original games.

I've also been looking at [GSRecomp](https://github.com/Shmargus/GSRecomp), which uses static recompilation to make a native port. Its launcher, progress transfer and automation are useful references. I considered building on it, but I'm continuing with Godot for now. Open Golden Sun isn't a fork of GSRecomp.

## Giving the agents smaller jobs

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

## Saving includes what happens next

Along with the player's location and inventory, a save needs to remember story flags and progression. It also needs the random-number state. Without that, saving halfway through a battle and restarting could change what happens next.

Each save also records which content pack it belongs to. Even changing a texture changes the pack's identity, so a save won't quietly load different content just because the area names happen to match.

The test content now runs through the main scene and can continue after closing and reopening the game. Basic physical battles carry HP, coins and experience back into the field. There's still a lot of combat left to implement.

The visual tests read pixels from the OpenGL viewport to check three static layers, how they overlap and whether the camera moves correctly. I also had the checks run with deliberately wrong pixel and camera expectations. Those failed, as they should. This round of visual work passed 48 Godot cases, 37 Python reference checks and 26 rendering, replay and rejection checks.

## Getting a real map through it

As of November 10, these tests use content made for the project. I haven't got a verified playthrough of an original region yet, or confirmed that the rendering matches the original pixels. The layer tests cover static base tiles before animation runs. Matching the original game's first visible frame still needs checking, as do the planned languages and platforms.

Next I want to convert the selected local map data, load it in the main scene and run the save/restart test there. The tool that builds the content pack has passed review, but the actual map trial is still pending. After that there's animation, collision, events and the original NPC behavior to work through.

The [project repository](https://github.com/Dewopalis/Open-Golden-Sun) is public, though I haven't pushed the restart work described here yet. Original ROMs, art, dialogue and converted original content stay local as well. The repository contains the reconstruction code, tools and authored test content.

For now, I want to get that one map through the whole process and see where it breaks next.
