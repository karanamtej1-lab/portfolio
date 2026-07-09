---
title: "Workflow Automation Tool"
date: 2026-01-20
summary: "Glue code, promoted: a tool that chains the repetitive steps I kept doing by hand."
tags: ["python", "automation"]
stack: ["Python", "YAML workflow specs", "Run-history store"]
---

<!-- TODO: replace with a real screenshot of the workflow/run-log view, e.g. /images/projects/workflow-tool-runs.png -->

## Overview

Started as three scripts and a cron job; became a small tool that chains
repetitive steps — fetch, transform, notify — into declarative workflows,
so I stop doing them by hand every time something changes.

## The problem

Every one of the three original scripts had slightly different logging,
slightly different failure handling, and zero shared history. When one
silently stopped working, I found out days later. The scripts weren't the
problem — the lack of a shared, observable way to run them was.

## Approach

- Workflows are defined declaratively in YAML: a list of steps, each a
  fetch/transform/notify action, run in order.
- Every run is logged with what ran, when, and the outcome of each step —
  not just a final success/failure, so a partial failure is diagnosable
  without re-running anything.
- Failures notify explicitly rather than failing silently into a log file
  nobody reads.


## Challenges & lessons

Automation you can't observe is automation you can't trust. The interesting
work here wasn't chaining the steps together — that part's easy — it was
building the "what ran, when, and why" view, which ended up being about half
the actual project. A workflow that fails and tells you exactly which step
and why is worth more than a workflow that rarely fails but goes silent when
it does.

## Status

Personal project, in everyday use for my own recurring tasks.
