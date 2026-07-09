---
title: "Workflow Automation Tool"
date: 2026-01-20
summary: "A small tool to chain the repetitive steps I kept doing by hand."
tags: ["python", "automation"]
stack: ["Python", "YAML workflow specs", "Run-history store"]
---

<!-- TODO: replace with a real screenshot of the workflow/run-log view, e.g. /images/projects/workflow-tool-runs.png -->

## Overview

This began as three scripts and a cron job. Now a small tool chains repetitive
steps, fetch, transform, notify, into workflows I define once. I stop running
them by hand every time something changes.

## The problem

The three scripts logged differently and handled failures differently. They
shared no history. When one broke, I found out days later. The scripts worked
fine. The missing piece was one shared, visible way to run them.

## How it works

- Workflows live in YAML. Each is a list of steps. Each step is a fetch,
  transform, or notify action. They run in order.
- Every run logs what ran, when, and how each step turned out. Not pass or fail
  alone. A partial failure traces easily with no full rerun.
- Failures tell you. They do not die in a log nobody reads.

## What I learned

You need to watch automation to trust automation. Chaining steps was easy. The
real work was the "what ran, when, and why" view. The view took about half the
project. A workflow which fails and names the step and reason beats one which
rarely fails and goes silent.

## Status

Personal project. Part of my daily routine.
