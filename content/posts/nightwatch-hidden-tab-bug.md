---
title: "The Nightwatch bug you only saw when you weren't looking"
date: 2026-06-20
summary: "The session timer drifted, but only for some people, and never while I debugged. Hidden tabs were the cause."
tags: ["debugging", "javascript"]
---

Nightwatch's session timer had a bug I failed to reproduce for a week. Sessions
ended late, sometimes minutes late, but only for some users. Never while I
watched.

The "never while I watched" detail was the whole bug.

## The setup

The timer UI updated with `requestAnimationFrame`. The countdown ran on chained
`setTimeout` ticks. Clean, smooth, passed every test. Every test shared one
trait. The tab sat in front of me.

## What browsers do in the background

A hidden tab triggers battery savings:

- `requestAnimationFrame` stops firing. The API ties to paint, and hidden tabs
  do not paint.
- `setTimeout` and `setInterval` throttle. Once per second at first. Less the
  longer the tab stays hidden.

So the moment a user switched tabs mid-session, my "one tick per second"
countdown became "one tick whenever the browser wants." The UI froze. Nobody
watches a hidden tab, so the freeze stayed invisible. The drift piled up until
they returned.

## The fix

Two changes. Both plain. Both defaults for me now.

1. Never count time by counting ticks. Save the end timestamp once. On every
   tick, compute `remaining = endTime - Date.now()`. Ticks throttle. The wall
   clock does not.
2. Re-sync on visibility change. A `visibilitychange` listener recomputes state
   the instant the tab returns. The UI reads correct on the first visible
   frame.

## The lesson

Your code counts on callbacks firing on schedule. The browser treats the
schedule as a suggestion. Anchor anything time-critical to timestamps. Give
anything visual a "you came back" path. A bug hidden under observation is
usually `document.hidden === true`.
