---
title: "Nightwatch"
date: 2026-05-10
summary: "A time tracker built to stay accurate even when you tab away."
tags: ["web", "javascript"]
stack: ["JavaScript", "Page Visibility API", "setTimeout / rAF"]
---

<!-- TODO: replace with a real screenshot of the timer/session UI, e.g. /images/projects/nightwatch-timer.png -->

## Overview

Nightwatch is a time tracker. Timed sessions, streaks, and enough pressure to
finish what you start.

## The problem

Most timers assume the tab stays in front of you the whole session. The
assumption breaks the second someone checks a message and returns. Nightwatch
keeps the real elapsed time across the interruption. More than a moving number
on screen.

## How it works

- Sessions anchor to a saved end-timestamp, not a tick counter. The countdown
  comes from the wall clock, not from how many timer callbacks fired.
- A `visibilitychange` listener re-syncs state the instant a hidden tab
  returns. The UI reads correct on the first visible frame.
- Streaks and history sit on top. The timer earned trust first.

## What I learned

My favorite bug lived here. Sessions ended minutes late, but only for some
users, and never while I watched. Browsers throttle `requestAnimationFrame` and
`setTimeout` in hidden tabs to save battery. My "one tick per second" countdown
became "one tick whenever the browser wants" the moment a tab dropped to the
background. Full story: [the hidden-tab bug
post](/posts/nightwatch-hidden-tab-bug/).

## Status

Personal project. Part of my daily routine.
