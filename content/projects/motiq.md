---
title: "Motiq"
date: 2026-04-02
summary: "A focus and productivity app — and the source of my favorite bug so far."
tags: ["web", "javascript"]
stack: ["JavaScript", "Page Visibility API", "setTimeout / rAF"]
---

<!-- TODO: replace with a real screenshot of the session/timer UI, e.g. /images/projects/motiq-timer.png -->

## Overview

Motiq is a focus/productivity app: timed sessions, streaks, and gentle
pressure to actually finish what you started instead of quietly switching
tabs.

## The problem

Most focus-timer apps assume the tab stays in the foreground the whole
session — an assumption that breaks the moment someone checks a message and
comes back. Motiq needed to stay *correct*, not just visually running,
across exactly that kind of interruption.

## Approach

- Sessions are anchored to a stored end-timestamp, not a tick counter, so
  the countdown is always derived from the wall clock rather than from how
  many timer callbacks actually fired.
- A `visibilitychange` listener reconciles state the instant a hidden tab
  becomes visible again, so the UI is correct on the very first visible
  frame instead of catching up over the next few ticks.
- Streaks and session history are the reward layer on top — the timer had
  to be trustworthy before any of that mattered.


## Challenges & lessons

The best bug I've hit so far lived here: sessions were ending minutes late,
but only for some users, and never while I was watching. The cause was
browsers throttling `requestAnimationFrame` and `setTimeout` in hidden tabs
to save battery — my "one tick per second" countdown became "one tick
whenever the browser felt like it" the moment a tab went to the background.
Full写-up: [the hidden-tab bug post](/posts/motiq-hidden-tab-bug/).

## Status

Personal project, used daily by me.
