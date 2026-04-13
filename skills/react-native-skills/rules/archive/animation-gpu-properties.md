---
id: animation-gpu-properties
title: Animate GPU Properties Only
priority: high
triggers:
  - reanimated slow
  - choppy animation
  - animate width react native
bad_patterns:
  - animating layout properties (width, height, flex)
  - using useNativeDriver: false
platforms:
  - ios
  - android
  - cross-platform
category: animation
---

### Problem
Animating properties like width/height triggers a layout pass for every frame on the main thread, causing frame drops.

### Why it matters
Animations must run at 60/120 FPS for a premium feel. Layout shifts prevent this.

### Bad Example
```tsx
const animatedStyle = useAnimatedStyle(() => ({
  width: withTiming(sv.value), // BAD: Triggers layout
}));
```

### Good Example
```tsx
const animatedStyle = useAnimatedStyle(() => ({
  transform: [{ scaleX: withTiming(sv.value) }], // GOOD: GPU only
}));
```
