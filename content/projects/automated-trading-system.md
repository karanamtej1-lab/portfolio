---
title: "Automated Trading System"
date: 2026-02-15
summary: "A Python system to backtest strategies and paper-trade them live."
tags: ["python", "finance"]
stack: ["Python", "pandas", "Broker API", "Backtesting engine"]
---

<!-- TODO: replace with a real chart/dashboard screenshot, e.g. /images/projects/trading-system-backtest.png -->

## Overview

A Python system takes a trading strategy from idea to paper execution. The
system pulls historical data, runs a backtest with realistic fills and fees,
and paper-trades live against real market data.

## The problem

Most first-attempt backtests lie to you. They fill every order instantly at
your exact price. They ignore fees. They let you peek at future data by
accident. An over-optimistic backtest is worse than none. A rosy backtest hands
you false confidence right before you risk real money.

## How it works

- Historical data loads and cleans once. Every strategy run starts from the
  same source.
- The backtest engine models realistic fills: slippage, partial fills, and
  fees. No perfect execution.
- A live paper-trading mode runs the same strategy code against a broker's
  paper account. Backtest and live share one logic, not two versions with
  drift.

## What I learned

The backtest was the easy part. The gap between backtest and live held every
hard problem. Real data arrives with delays the backtest ignores. Real orders
fill partially in ways a sim skips. The biggest risk was me wanting to tweak
one parameter after a bad day. One shared code path removed the risk. Nothing
was left to change in secret.

## Status

Personal project. Paper trading only.
