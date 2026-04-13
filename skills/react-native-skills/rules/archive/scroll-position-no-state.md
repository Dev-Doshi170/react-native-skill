---
id: scroll-position-no-state
title: Avoid Scroll Position in State
priority: high
triggers:
  - slow header animation
  - scroll listener lag
  - onScroll performance
bad_patterns:
  - updating React state on every scroll event
platforms:
  - ios
  - android
  - cross-platform
category: performance
---

### Problem
Updating local state (setState) inside an onScroll listener triggers a re-render for every pixel scrolled.

### Why it matters
The re-render cycle is too slow for 60FPS. The UI will lag behind the finger.

### Good Example
Use useAnimatedScrollHandler from Reanimated to handle scroll events on the UI thread.
