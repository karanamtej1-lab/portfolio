---
title: "Recut"
date: 2026-06-01
summary: "A mobile app for remixing viral Instagram/TikTok video templates — browse trending formats, drop in your own clips, export."
tags: ["react-native", "expo", "mobile"]
stack: ["React Native", "Expo (SDK 56)", "Expo Router", "Node.js", "FFmpeg", "AsyncStorage"]
---

## Overview

Recut lets you remix trending short-form video formats without touching a real
editor. Browse the formats blowing up right now — "Day In My Life,"
multi-clip transition edits — pick one, drop in your own clips, and export.

![Discover screen — trending templates by category](recut-app.png "Recut's Discover screen")

## How it works

- **React Native + Expo** for the app and camera/media access, so one
  codebase runs on iOS, Android, and web.
- **A Node + FFmpeg server** does the actual video work — stitching clips to
  a template's timing, transitions, and audio sync. That's real video
  processing, not something you fake in a WebView.
- Templates are grouped by category (Trending, Travel, Fitness, Food,
  Fashion) with engagement counts, so browsing feels like scrolling a feed
  instead of picking files.

## What I learned

FFmpeg in a managed hosting environment is way more fragile than it looks. The
build that ships in some Node images is missing codecs the audio/video sync
depends on, so rendering would randomly break. The fix was being explicit
about exactly which FFmpeg build the server runs instead of trusting whatever
the platform hands you. It also locked in the architecture: render on the
server, never in the client — mobile WebViews just can't do frame-accurate
video.

## Status

In active development.
