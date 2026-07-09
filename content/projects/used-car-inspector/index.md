---
title: "Used-Car Inspector"
date: 2026-07-02
summary: "An AI condition report for a used car listing — flags with evidence, a price verdict against real comps, and the questions to ask before you buy."
tags: ["nextjs", "ai", "react"]
stack: ["Next.js (App Router)", "React", "Vision LLM API", "Hand-written CSS", "PWA"]
---

## Overview

Buying a used car from a private seller means trusting someone who knows
exactly what's wrong with it and has zero reason to tell you. Used-Car
Inspector closes that gap. Upload the listing photos and you get condition
flags with actual evidence (not just a confidence number), a fair-price range
backed by real comparable listings, and the specific questions to ask the
seller.

![Home screen — condition score, quick actions, recent inspections](used-car-inspector-home.png "Home dashboard")

## The core problem

Most "AI inspection" demos just spit out a percentage and call it a day —
"87% confidence: damage detected." That's not trustworthy, it just sounds
confident. If the model can't point at *why* it thinks something's wrong, the
number is decoration. So the whole rule became: every claim has to show its
work.

![A real report — condition flags with cited evidence and a price verdict against comps](used-car-inspector-report.png "Condition report with evidence")

## How it works

- **Two AI calls, not one black box.** A vision pass reads the photos against
  a fixed checklist (rust, panel misalignment, tire wear, warning lights,
  interior wear) and has to say "not assessable" instead of guessing when a
  photo doesn't show something clearly. A second pass takes those flags plus
  real comps and works out the price — it can't make up a number with no
  comps behind it.
- **The condition score is a formula, not a model output.** Starts at 100,
  subtracts a fixed amount per flag based on confidence. The math is right
  there on every report. No mystery score.
- **Local-first.** Inspections save to your browser, not a server. No account,
  nothing to leak.

## What I learned

The hard part wasn't the AI call — it was getting the AI to say "I don't
know." Models love to sound confident, so getting the vision pass to reliably
flag "not assessable" on a blurry or weird-angle photo took a bunch of prompt
tightening and a validation step on the output before I trusted it enough to
ship.

## Status

Actively developed. Source is private for now.
