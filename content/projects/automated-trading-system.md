---
title: "Automated Trading System"
date: 2026-02-15
summary: "Python system that backtests strategies and paper-trades them live."
tags: ["python", "finance"]
stack: ["Python", "pandas", "Broker API", "Backtesting engine"]
---

<!-- TODO: replace with a real chart/dashboard screenshot, e.g. /images/projects/trading-system-backtest.png -->

## Overview

A Python system that takes a trading strategy from idea to (paper)
execution: historical data ingestion, a backtesting loop with realistic
fills and fees, and a live paper-trading mode against real market data.

## The problem

Most first-attempt backtests lie to you. They fill every order instantly at
the exact price you asked for, ignore transaction costs, and let you
accidentally peek at future data. A backtest that's too optimistic is worse
than no backtest, because it gives you false confidence right before you
risk real money.

## Approach

- Historical data is ingested and normalized once, so every strategy run
  works from the same clean source.
- The backtest engine simulates realistic fills (slippage, partial fills)
  and fees, rather than assuming perfect execution.
- A separate live paper-trading mode runs the same strategy code against a
  broker's paper account, so the backtest and the live run are provably the
  same logic, not two implementations that can drift apart.


## Challenges & lessons

The backtest was the easy part. Everything hard lived in the gap between
backtest and live: real data arrives with delays the backtest doesn't
model, real orders get partially filled in ways a simulation can gloss
over, and the biggest risk wasn't a bad strategy — it was the temptation to
"just tweak one parameter" after a losing day. Keeping backtest and live
sharing one code path removed a whole category of that self-deception,
because there was nothing left to secretly change.

## Status

Personal project; paper-trading only.
