---
title: "Nightwatch"
date: 2026-05-10
summary: "A monitoring tool that watches the things I'd otherwise forget to check."
tags: ["python", "automation"]
stack: ["Python", "Scheduler", "Flat-file storage", "Email / webhook alerts"]
---

<!-- TODO: replace with a real screenshot once Nightwatch has a UI/dashboard worth showing, e.g. /images/projects/nightwatch-dashboard.png -->

## Overview

Nightwatch is a small monitoring tool: point it at the things you care about
— endpoints, files, scheduled jobs — and it checks them on a schedule and
tells you the moment something drifts from what it should look like.

## The problem

The things that break quietly are worse than the things that break loudly.
A crashed job that emails you is annoying; a job that "succeeds" but writes
garbage for three days before anyone notices is a real problem. Nightwatch
exists for the second category.

## Approach

- A scheduler runs checks at configurable intervals against a small set of
  check types (HTTP status/latency, file freshness, job exit codes).
- State is stored plainly — no database server to babysit — so the tool
  itself is one more thing that can't quietly break.
- Alerts are deliberately rate-limited per check, so a flapping endpoint
  doesn't spam you into ignoring everything.


## Challenges & lessons

Alerting turned out to be a design problem, not a plumbing problem. An alert
that fires too often trains you to ignore it, which is strictly worse than
no alert at all — so most of the actual design work went into *when to stay
quiet*, not into detecting problems in the first place.

## Status

Personal project, actively iterated on. Repo link coming once it's cleaned
up for public viewing.
