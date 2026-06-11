# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A collection of archived SMS-style conversation threads published as supporting reference
material for reviews on mrnumber.com (a reverse phone-lookup / caller-reputation site). Each
thread reproduces a text-message exchange with a particular phone number, rendered as a dark,
self-contained "messaging app" web page.

There is **no build system, package manager, test suite, or backend**. Every file is plain
static HTML meant to be opened directly in a browser or served as a static page (e.g. GitHub
Pages). To preview a thread, just open the `.html` file in a browser — there is nothing to
compile or install.

## File conventions

- Each conversation thread lives in its own standalone `.html` file. The filename is the
  10-digit phone number of the thread (e.g. `9175876707.html`). `index.html` is the default
  landing thread.
- Files are **fully self-contained**: all CSS is inlined in a single `<style>` block in the
  `<head>`, and the only external dependency is Google Fonts (Syne, JetBrains Mono, Lora)
  loaded via CDN `<link>`. Do not introduce a separate stylesheet, framework, or JS bundle —
  keep new threads consistent with this single-file pattern.

## Thread page structure

Each page follows the same anatomy. When creating a new thread, copy an existing file and
replace the content rather than building markup from scratch:

- `:root` CSS custom properties define the whole palette (`--bg`, `--surf*`, `--gold*`,
  `--text*`, etc.). Restyle via these tokens, not hardcoded colors.
- `.top-bar` — header showing the `.avatar` (area code), `.contact-name` (the phone number),
  `.contact-num` (location · area code), and `.msg-count` (keep this in sync with the actual
  number of message bubbles).
- `.note-banner` — the fixed disclaimer stating the conversation was archived as a public
  record / posted as reference on mrnumber.com.
- `.thread` — the scrollable message pane. Inside it:
  - `.date-divider` — a centered date/time separator between message groups.
  - `.bubble-wrap` with modifier `inc` (incoming, left-aligned, neutral surface) or `out`
    (outgoing, right-aligned, gold). Each wrap contains a `.bubble` (`inc`/`out` to match) and
    a `.bubble-time`. **`inc`/`out` must match between the wrap and the bubble.**
  - `.delivered` — final read-receipt line at the bottom of the last outgoing message.
- A small inline `<script>` at the end auto-scrolls the thread to the bottom on load
  (`w.scrollTop = w.scrollHeight`).

## Editing notes

- `index.html` and `9175876707.html` are two renderings of the **same** 917-587-6707
  conversation; they intentionally differ in timestamps and message count. When updating the
  thread, check whether both files should change to stay consistent.
- `<strong>` is used inline to emphasize addresses and key details within a bubble.
- The `--msg-count` text in `.top-bar` is manual — update it when adding/removing bubbles.

## Git workflow

- The published/stable branch is `main`.
- The repo is intended to be hosted as static pages, so any committed `.html` change is
  effectively a deploy — preview in a browser before committing.
