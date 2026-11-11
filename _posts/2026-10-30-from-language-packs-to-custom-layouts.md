---
layout: post
title: "From language packs to editable layouts in Daisywheel"
date: 2026-10-30 17:00:00 +0200
categories: [workbench]
tags: [daisywheel, accessibility, controllers]
excerpt: "A question about typing ê on a controller led from language packs to a small text format anyone can edit."
---

This started with a small question: could Daisywheel handle characters that do not fit on its standard wheel?

I've been practicing with the wheel, trying to build the muscle memory to type faster with a controller than I can on a regular on-screen QWERTY keyboard. I've used it on desktop apps like Telegram when chatting to my brothers and that is where I realized that there was a gap. We speak Afrikaans and characters like "ê" show up in ordinary words and my keyboard could not type this out. The patriot in me believed it was unacceptable, so I had to do something about it.

My first thought was some kind of language pack. That sounded like a neat feature, but the more I thought about it, the less the language-pack idea fit.

## The language pack idea got too specific

When I started listing the characters I actually wanted, it became obvious that they were not all unique to Afrikaans. Accented letters such as á, è, ë, and î are useful across different languages. Calling the feature an Afrikaans layout would make it sound more specific than it was, and adding a separate built-in pack for every language would quickly turn into a maintenance project of its own.

So I changed the name to **Extended special characters**. The built-in layout adds another page to the wheel for characters and punctuation that are missing from the standard layout. That gives me somewhere useful to start without pretending it is a complete keyboard for any particular language.

## How configurable should it be?

The next idea was to let people configure every letter and symbol. That could become a full editor inside Daisywheel, with every slot editable through the controller. It is a cool idea, especially for a controller-first app, but it would also make the settings much more complicated and I think it would exceed the scope I had in mind for a first release.

For this first pass, I wanted to keep the feature smaller. Configuring dozens of character slots using a controller would be a project by itself. I decided that importing and exporting a layout file was a better starting point. It is less ambitious, but it lets people make a layout that fits them without making the settings screen into a layout editor.

## Who is JSON, and why is he in my text file?

The first file-format idea was JSON. It is common, structured, and straightforward for software to read. But when I looked at what the initial JSON export looked like, a nested collection of brackets and quoted values is not something a non-technical person would be super comfortable editing.

I settled on a plain UTF-8 text file instead. Exporting a layout gives you a complete template that can be opened in Notepad, Kate, Gedit, or basically any other text editor. Each section names a page and case, and each petal row lists four characters in A, B, X, Y order. You can edit the characters and import the file again.

The file also has a name, which appears in Daisywheel's layout selector. The first line says `DAISYWHEEL LAYOUT V1`. I initially used a less obvious version marker, and it was not clear why that line mattered. The `V1` makes it explicit that it identifies the file format, so Daisywheel can distinguish this layout structure from a future one. It is not a setting people need to customize.

## Validating to stop people from breaking things

Importing a text file means Daisywheel cannot assume that every file is complete or correctly formatted. The importer checks the version marker, the required sections and petal rows, and that each slot contains one Unicode character. It also rejects duplicate or unknown sections and keeps the file size bounded. If something is wrong, it reports the problem without replacing a layout that is already saved.

There is a limit to this format: a slot holds one Unicode code point. Precomposed characters such as `ê` work, while a character built from a base letter plus a separate combining mark does not. That is a deliberate boundary for this version, and something to revisit if real layouts need it.

I also decided to cap the saved collection arbitrarily at eight custom layouts. That is enough room to keep a few language or personal layouts around, while keeping the selector manageable. Importing another layout with the same name replaces the existing one, and there is a control to remove a saved layout.

## The first idea is not always the best one.

I'm glad I explored the language-pack idea and played around with JSON. It showed me that I had the right idea but my initial execution just sucked.

For now, people can edit and share layouts using a text editor. I want to see what they make before tackling an editor inside Daisywheel.
