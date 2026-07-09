---
title: "Workflow Automation Tool"
date: 2026-01-20
summary: "Glue code, promoted: a tool that chains the repetitive steps I kept doing by hand."
tags: ["python", "automation"]
stack: ["Python", "YAML workflow specs", "Run-history store"]
---

<!-- TODO: replace with a real screenshot of the workflow/run-log view, e.g. /images/projects/workflow-tool-runs.png -->

## Overview

Started as three scripts and a cron job. Turned into a small tool that chains
repetitive steps — fetch, transform, notify — into workflows I define once, so
I stop doing them by hand every time something changes.

## The problem

Each of the three original scripts logged things a little differently, handled
failures a little differently, and shared zero history. When one quietly
stopped working, I'd find out days later. The scripts weren't the problem —
the lack of one shared, visible way to run them was.

## How it works

- Workflows are written in YAML: a list of steps, each one a
  fetch/transform/notify action, run in order.
- Every run logs what ran, when, and how each step turned out — not just
  pass/fail, so a partial failure is easy to trace without re-running
  everything.
- Failures actually tell you instead of dying quietly in a log nobody reads.

## What I learned

Automation you can't watch is automation you can't trust. The interesting work
wasn't chaining steps together — that part's easy — it was building the "what
ran, when, and why" view, which ended up being about half the project. A
workflow that fails and tells you exactly which step and why beats one that
rarely fails but goes silent when it does.

## Status

Personal project, I use it every day for my own recurring tasks.
