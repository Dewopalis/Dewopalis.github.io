---
layout: post
title: "When the macOS build hung before main"
date: 2026-10-22 18:30:00 +0200
categories: [workbench]
tags: [daisywheel]
featured: true
excerpt: "The Mac build compiled and packaged successfully, then appeared to freeze. Finding the cause meant looking past the build log and into the app bundle."
---

I don't have a Mac, so the first version of Daisywheel's macOS build had to be
developed through GitHub Actions. That made the CI runner my test machine. When
the packaging job started timing out, I had to work out whether the problem was
the build, the bundle, or simply the way the app was being tested.

The strange part was that most of the job looked healthy. The project compiled,
the tests passed, and the packaging script produced an app bundle. The log had
some alarming-looking Qt library-path messages, but those were not where the
job stopped. The failure came later, when the workflow tried to launch the
packaged app as a smoke test. It sat there until the timeout.

## A successful build can still make a broken app

At first, a timeout did not tell me much. The app could have been waiting for
input, failing to find a plugin, or stuck in some startup code. Since I could
not watch the Mac runner's screen, I changed the smoke test to collect a process
sample if the app took too long to start.

That sample gave us the important clue: the main thread was inside Apple's
`NSAlert` modal-dialog code, reached from SDL while dynamic libraries were
initializing. The app had not made it to `main()` yet. This was not Daisywheel
waiting for a controller or a window. Something lower in the startup chain was
showing a system dialog that CI could not interact with.

## The missing library was not obvious

The Mac runner installs SDL2 through Homebrew. In this environment, that SDL2
package uses SDL2-compat, a compatibility layer that forwards SDL2 calls to
SDL3. It loads SDL3 dynamically at runtime. That detail matters because a
packaging tool can discover ordinary linked libraries, but it cannot always
see a library that another library finds later with `dlopen`.

`macdeployqt` had bundled the SDL2 compatibility library, but not the SDL3
library it needed. When SDL2-compat could not find SDL3 in the packaged app,
startup went into the modal alert path. On a developer's desktop, a dialog like
that might be visible and dismissible. In a background CI job, it just looks
like a hung process.

The fix was to copy `libSDL3.dylib` into the app's frameworks directory beside
the bundled SDL2 library and set its install name to a path relative to that
directory. The packaging script also checks that the copied library does not
still point back to Homebrew paths. After that, the smoke test could launch the
app both before and after ad-hoc signing, and the workflow produced its DMG and
ZIP artifacts.

## What I learned from the timeout

The first lesson was that “it built” and “the packaged app starts” are separate
claims. A compile and passing unit tests tell me the source is in good shape.
They do not prove that the final bundle contains every runtime dependency.

The second lesson was to make CI explain a timeout. A bare timeout only says
that something did not finish. Capturing a process sample turned a vague
failure into a specific startup path, and helped us distinguish the real issue
from messages that happened to look like errors in the packaging output.

There is still a limit to what this proves. The macOS runner can build and
launch the app, but I still need someone with a Mac to try the actual keyboard
and desktop behavior. CI can catch missing libraries and packaging regressions;
it cannot replace testing on the platform itself.

I expected the hard part of adding macOS support to be adapting the application
code. For this failure, the source code was not the problem. The difficult part
was understanding how a dependency was loaded, what the bundle contained, and
why a background launch behaved differently from a normal desktop launch.
