---
title: "Automated Trading System"
date: 2026-02-15
summary: "Python system that backtests strategies and paper-trades them live."
tags: ["python", "finance"]
stack: ["Python", "pandas", "Broker API", "Backtesting engine"]
---

<!-- TODO: replace with a real chart/dashboard screenshot, e.g. /images/projects/trading-system-backtest.png -->

## Overview

A Python system that takes a trading strategy from idea all the way to (paper)
execution: it pulls in historical data, runs a backtest with realistic fills
and fees, and has a live paper-trading mode against real market data.

## The problem

Most first-attempt backtests lie to you. They fill every order instantly at
the exact price you wanted, ignore fees, and let you accidentally peek at
future data. A backtest that's too optimistic is worse than no backtest — it
hands you false confidence right before you put real money on the line.

## How it works

- Historical data gets pulled in and cleaned up once, so every strategy run
  starts from the same source.
- The backtest engine fakes realistic fills (slippage, partial fills) and fees
  instead of assuming everything's perfect.
- A separate live paper-trading mode runs the *same* strategy code against a
  broker's paper account, so the backtest and the live run are provably the
  same logic — not two versions that quietly drift apart.

## What I learned

The backtest was the easy part. Everything hard lived in the gap between
backtest and live: real data shows up with delays the backtest doesn't model,
real orders fill partially in ways a sim glosses over, and the biggest risk
wasn't a bad strategy — it was me wanting to "just tweak one parameter" after
a bad day. Keeping backtest and live on one shared code path killed a whole
category of that self-deception, because there was nothing left to secretly
change.

## Status

Personal project, paper-trading only.
