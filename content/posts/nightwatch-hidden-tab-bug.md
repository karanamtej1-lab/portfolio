---
title: "The Nightwatch bug that only showed up when you weren't looking"
date: 2026-06-20
summary: "The session timer kept drifting — but only for some people, and never while I was debugging. Turned out hidden tabs were the whole story."
tags: ["debugging", "javascript"]
---

Nightwatch's session timer had a bug I couldn't reproduce for a week. Sessions
were ending late — sometimes *minutes* late — but only for some users, and
never, ever while I was watching.

That last part turned out to be the whole bug.

## The setup

The timer UI updated with `requestAnimationFrame`, and the countdown ran on
chained `setTimeout` ticks. Clean, smooth, passed every test. But all my tests
had one thing in common: the tab was right in front of me.

## What browsers do behind your back

When a tab is hidden, browsers get aggressive about saving battery:

- `requestAnimationFrame` **stops firing completely** — it's tied to painting,
  and hidden tabs don't paint.
- `setTimeout` / `setInterval` get **throttled** — down to once a second at
  first, and way less the longer the tab stays backgrounded.

So the moment someone switched tabs mid-session, my "one tick per second"
countdown quietly turned into "one tick whenever the browser feels like it."
The UI froze — invisibly, since nobody's staring at a hidden tab — and the
drift piled up until they came back.

## The fix

Two changes, both boring, both things I just do by default now:

1. **Never count time by counting ticks.** Save the session's end timestamp
   once, and on every tick compute `remaining = endTime - Date.now()`. Ticks
   can be throttled; the wall clock can't.
2. **Re-sync on visibility change.** A `visibilitychange` listener recomputes
   everything the instant the tab comes back, so the UI is correct on the very
   first visible frame — no waiting around for the next tick.

## The lesson

If your code counts on callbacks firing on schedule, the browser treats that
as a suggestion, not a promise. Anything time-critical should be anchored to
timestamps, and anything visual needs a "hey, you're back" path. Bugs that
vanish when you look at them aren't spooky — usually it's just
`document.hidden === true`. 😁
