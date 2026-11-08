---
layout: post
title: "Getting an 11-year-old niche controller working with Wi-Fi Direct"
date: 2026-11-02 12:00:00 +0200
categories: [workbench]
tags: [linux, hardware, networking, reverse-engineering]
featured: true
excerpt: "I wanted to use the original Wi-Fi-only NVIDIA SHIELD Controller on Linux. Getting it onto the network was the easy part."
---

I have been tinkering with Daisywheel for a while, but every so often a different project grabs my attention. This time it was my old NVIDIA SHIELD Controller, the original 2014/2015 or "fat" model.

I wanted to see if I could get it working wirelessly on Linux. Back in the day, I couldn't get this controller working on my Linux PC with its AMD graphics card, even on a wired connection, so I knew there would be a few hurdles. The catch is that this controller does not use Bluetooth. It connects over Wi-Fi Direct, so the project quickly turned into a question about Linux networking, an old kernel driver, and what it actually means for a controller to be connected.

## The first plan was to investigate the Windows driver

I initially thought I would need to reverse engineer NVIDIA's Windows support. I started looking at the available references and protocol evidence. REA helped me make sense of the open-source code I was reading, and TShark helped me inspect the traffic.

The useful foundation turned out to be closer to home: an old OZWPAN USB-over-Wi-Fi driver that had lived in the Linux staging tree, plus an open-source Windows USB driver reference. I restored and adapted the Linux driver for this controller and current kernels. NVIDIA's Android HID driver and observed controller reports also helped me understand the touchpad behavior.

I did not decompile or redistribute an NVIDIA Windows binary. This became a Linux driver port and protocol investigation, based on open-source code and observed behavior.

## Pairing was the easy part

My first tests could get the controller to associate with the computer. That felt like a win, until the temporary Wi-Fi Direct group disappeared or my PC lost its normal Wi-Fi connection. I could make the controller connect, but the setup was not something I would want to use day to day.

The problem was making the controller's network coexist with the Wi-Fi connection I was already using. The working setup creates a separate virtual access point interface called `shield0` on the same channel as the connected 2.4 GHz Wi-Fi interface. A small system service runs the controller access point there, while NetworkManager keeps managing the normal Wi-Fi connection.

That lets the controller and my usual Wi-Fi share one adapter without FatShield taking over the whole network connection. The adapter has to support running a managed connection and an access point on the same channel. The service waits if the current connection is on 5 or 6 GHz rather than changing my Wi-Fi settings for me. In other words, it can share the connection I already have, but it won't quietly rearrange my network to make the controller work.

## Connected did not mean sending input

The next false finish was seeing Linux create the controller's input devices. The gamepad and mouse nodes appeared, so I expected button presses to show up. They did not.

I recorded the evdev events to find out where the chain stopped. The devices were enumerated, but HID reports were not streaming. In the tested sequence, opening the virtual controller's hidraw device and issuing read-only `HIDIOCGINPUT` requests for reports 1 and 2 got the stream moving. I tested opening hidraw and reading the reports together, so I have not isolated which part was essential. The fix uses reads, not undocumented HID writes.

There was also a more obvious driver problem: a USB descriptor callback reported how many bytes it had been asked to copy, rather than how many it actually copied. That eventually caused a kernel fault during enumeration. Fixing the byte counts, bounds, and fragmented responses turned out to matter just as much as understanding the controller protocol.

The touchpad had its own small surprise. It reports absolute coordinates even though its mouse descriptor says relative motion. FatShield tracks the previous coordinates and converts the movement into bounded relative mouse deltas, while filtering the jumps that can happen when a finger touches or leaves the pad.

## Making it usable outside a test script

Once input was working, I wanted something more useful than a collection of shell commands. Open FatShield now has a system service, a per-user desktop helper for volume and mute, and a small Qt tray and pairing window. The service owns the controller network interface, and the desktop helper maps media keys to the logged-in user's audio controls.

Packaging was another step toward making this a real project. The project builds Debian, RPM, and Arch packages. They include the DKMS source, so the kernel module is built on the user's computer for its installed kernel instead of shipping one precompiled module that only works with a particular kernel.

The project is currently an experimental alpha. The live hardware testing so far has been on CachyOS with an Intel iwlwifi adapter, 2.4 GHz Wi-Fi, and kernel 7.3.1. In one 60-second concurrent test, the controller produced 1,286 button, axis, and mouse events while normal Wi-Fi stayed connected. I have also confirmed rumble on this setup.

Those are encouraging results, but they do not mean every Linux computer is supported. The driver builds against 6.18 LTS headers, but runtime testing on that kernel is still pending. Other adapters and distributions need hardware testing too. I have not validated startup after a host reboot or reconnect after power-cycling the controller, and the restored kernel driver has not had a complete security audit.

## A much bigger rabbit hole than I expected

I started out thinking this would mostly be about figuring out a wireless controller protocol. Instead, the difficult parts were the boundaries between things: Wi-Fi Direct and NetworkManager, virtual USB devices and HID reports, absolute touchpad coordinates and relative mouse input, kernel code and a desktop application.

The surprise was that audio input and output worked too. I can use this silly chonk of a controller wirelessly for sound, which I absolutely did not expect to be one of the fun parts of the project.

## Finally, does it work with Daisywheel?

Yes! Once the controller connected, Daisywheel picked it up immediately in my test, including its name. Seeing the two projects meet like that was a very satisfying little moment.

<figure class="project-demo" style="max-width: 436px;">
  <img src="{{ '/assets/images/open-fatshield-daisywheel.png' | relative_url }}" alt="Daisywheel open with the NVIDIA SHIELD Controller detected and the symbols wheel visible" width="436" height="450">
  <figcaption>Daisywheel recognized the NVIDIA SHIELD Controller and showed its symbols wheel.</figcaption>
</figure>

You can find the code, package downloads, and current test boundaries in [Open FatShield on GitHub](https://github.com/Dewopalis/Open-FatShield). The [v0.2.0-alpha.1 release](https://github.com/Dewopalis/Open-FatShield/releases/tag/v0.2.0-alpha.1) has Debian, RPM, and Arch packages. It is still early, but the original controller is now usable wirelessly on my tested setup, without disconnecting normal Wi-Fi. That is a satisfying result for a side project that began with an old controller and a question.
