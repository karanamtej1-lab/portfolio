---
title: "Used-Car Inspector"
date: 2026-07-02
summary: "An AI condition report for a used-car listing. Flags with evidence, a price verdict against real comps, and the questions to ask before you buy."
tags: ["nextjs", "ai", "react"]
stack: ["Next.js (App Router)", "React", "Vision LLM API", "Hand-written CSS", "PWA"]
---

## Overview

Buy a used car from a private seller and you trust someone who knows the flaws
and has no reason to share them. Used-Car Inspector closes the gap. Upload the
listing photos. Get condition flags with evidence, not a bare confidence
number. Get a fair-price range from real comparable listings. Get the exact
questions to ask the seller.

![Home screen, condition score, quick actions, recent inspections](used-car-inspector-home.png "Home dashboard")

## The core problem

Most AI inspection demos print a percentage and stop. "87% confidence: damage
detected." A number with no evidence is decoration. So I set one rule. Every
claim shows evidence.

![A real report, condition flags with cited evidence and a price verdict against comps](used-car-inspector-report.png "Condition report with evidence")

## How it works

- Two AI calls, each with a clear job. A vision pass reads the photos against a
  fixed checklist: rust, panel misalignment, tire wear, warning lights,
  interior wear. The pass flags "not assessable" instead of guessing when a
  photo stays unclear. A second pass takes the flags plus real comps and sets
  the price. No comps, no number.
- The condition score is a formula, not a model output. Start at 100. Subtract
  a fixed amount per flag by confidence. The math shows on every report.
- Local-first. Inspections save to your browser, not a server. No account.
  Nothing to leak.

## What I learned

The hard part was making the AI say "I don't know." Models default to confident
answers. Getting the vision pass to flag "not assessable" on a blurry or
bad-angle photo took rounds of prompt tightening and a validation step on the
output.

## Status

In active development. Source stays private for now.
