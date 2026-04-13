---
id: ui-expo-image
title: Unified Expo Image standard
priority: high
triggers:
  - image performance
  - image caching react native
  - avoid image flicker
bad_patterns:
  - using native Image for remote URLs
  - missing placeholder/loading states
platforms:
  - ios
  - android
  - cross-platform
category: ui
---

### Problem
Standard React Native `Image` has poor caching on Android and lacks high-level features like transitions and blurhash support.

### Why it matters
Leads to flickering during loads and higher network usage as images are re-fetched unnecessarily.

### Bad Example
```tsx
import { Image } from 'react-native';
<Image source={{ uri }} /> // Slow, no caching on Android by default
```

### Good Example
```tsx
import { Image } from 'expo-image';
<Image
  source={uri}
  placeholder={blurhash}
  transition={300}
  contentFit="cover"
/>
```
