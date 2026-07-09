---
title: "The Motiq bug that only happened when you weren't looking"
date: 2026-06-20
summary: "Focus timers kept drifting — but only for some users, and never while I was debugging. The culprit: hidden tabs."
tags: ["debugging", "javascript"]
---

Motiq's focus timer had a bug I couldn't reproduce for a week: sessions were
ending late — sometimes *minutes* late — but only for some users, and never,
ever, while I was watching it.

That last detail turned out to be the whole bug.

## The setup

The timer UI updated with `requestAnimationFrame`, and the session countdown
used chained `setTimeout` ticks. Clean, smooth, worked perfectly in every
test. But my tests all had one thing in common: the tab was **in front of
me**.

## What browsers do behind your back

When a tab is hidden, browsers get aggressive about saving battery:

- `requestAnimationFrame` **stops firing entirely** — it's tied to paint,
  and hidden tabs don't paint.
- `setTimeout` / `setInterval` get **throttled** — to once per second at
  first, and much less than that for deeper background states.

So the moment a user switched tabs mid-session, my "one tick per second"
countdown quietly became "one tick whenever the browser felt like it." The
UI froze (invisibly — nobody's looking at a hidden tab), and the drift
accumulated until they came back.

## The fix

Two changes, both boring, both things I now do by default:

1. **Never count elapsed time by counting ticks.** Store the session's end
   timestamp once, and on every tick compute `remaining = endTime - Date.now()`.
   Ticks can be throttled; the wall clock can't.
2. **Reconcile on visibility change.** A `visibilitychange` listener
   recomputes state the instant the tab comes back, so the UI is correct on
   the very first visible frame — no waiting for the next tick.

## The lesson

If your code depends on callbacks firing on schedule, the browser considers
that a suggestion, not a contract. Anything time-critical needs to be
anchored to timestamps, and anything visual needs a "you just came back"
path. Bugs that hide when observed aren't spooky — they're usually just
`document.hidden === true`.
