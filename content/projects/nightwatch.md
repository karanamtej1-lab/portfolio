---
title: "Nightwatch"
date: 2026-05-10
summary: "A time tracker built to stay accurate even when you tab away."
tags: ["web", "javascript"]
stack: ["JavaScript", "Page Visibility API", "setTimeout / rAF"]
---

<!-- TODO: replace with a real screenshot of the timer/session UI, e.g. /images/projects/nightwatch-timer.png -->

## Overview

Nightwatch is a time tracker — timed sessions, streaks, and just enough
pressure to actually finish what you started instead of drifting off.

## The problem

Most timers assume the tab stays in front of you the whole session. That falls
apart the second someone checks a message and comes back. Nightwatch had to
stay *correct*, not just look like it's running, across exactly that kind of
interruption.

## How it works

- Sessions are anchored to a saved end-timestamp, not a tick counter, so the
  countdown always comes from the wall clock instead of however many timer
  callbacks actually fired.
- A `visibilitychange` listener re-syncs everything the instant a hidden tab
  comes back, so the UI is right on the very first visible frame instead of
  catching up over the next few ticks.
- Streaks and history sit on top as the fun part — but the timer had to be
  trustworthy first.

## What I learned

My favorite bug so far lived here: sessions were ending minutes late, but only
for some users, and never while I was watching. Turned out browsers throttle
`requestAnimationFrame` and `setTimeout` in hidden tabs to save battery, so my
"one tick per second" countdown became "one tick whenever the browser feels
like it" the moment a tab went to the background. Full story:
[the hidden-tab bug post](/posts/nightwatch-hidden-tab-bug/).

## Status

Personal project, I use it every day.
