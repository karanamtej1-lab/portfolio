---
title: "Recut"
date: 2026-06-01
summary: "A mobile app for remixing viral Instagram/TikTok video templates — browse trending formats, drop in your own clips, export."
tags: ["react-native", "expo", "mobile"]
stack: ["React Native", "Expo (SDK 56)", "Expo Router", "Node.js", "FFmpeg", "AsyncStorage"]
---

## Overview

Recut is a template-remixing app: browse trending short-form video formats
("Day In My Life," multi-clip transition edits), pick one, swap in your own
footage, and export — without learning a video editor.

![Discover screen — trending templates by category](recut-app.png "Recut's Discover screen")

## Approach

- **React Native + Expo** for the app shell and camera/media-library access,
  so the same codebase targets iOS, Android, and web from one project.
- **A Node/FFmpeg server** handles the actual video composition server-side
  — stitching clips to a template's timing, transitions, and audio sync is
  real video processing, not something to do in a WebView.
- Templates are organized by category (Trending, Travel, Fitness, Food,
  Fashion) with engagement counts so browsing feels like scrolling a feed,
  not a file picker.


## Challenges & lessons

FFmpeg in a serverless/managed environment is more fragile than it looks —
the version that ships in some Node hosting images is stripped of codecs a
template's audio/video sync depends on. Getting predictable rendering meant
being explicit about which FFmpeg build the server runs, not trusting
whatever the platform provides by default. It also pushed the architecture
firmly toward "render on the server, never in the client" once it was clear
mobile WebViews can't reliably handle frame-accurate video composition.

## Status

In active development.
