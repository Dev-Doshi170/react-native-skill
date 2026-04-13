---
id: list-performance-images
title: Optimizing Images in Lists
priority: high
triggers:
  - image lag in list
  - flatlist memory image
  - optimize images react native
bad_patterns:
  - loading full resolution images in small thumbnails
  - using default Image component for large lists
platforms:
  - ios
  - android
  - cross-platform
category: performance
---

### Problem
Loading many large images in a scrollable list consumes significant memory (RAM) and can cause the app to crash or stutter.

### Why it matters
The UI thread waits for image decoding, leading to dropped frames during scrolling. Unoptimized images are the #1 cause of "out of memory" errors.

### Bad Example
```tsx
import { Image } from 'react-native';

const ListItem = ({ item }) => (
  // Loads entire 4K image for a 50x50 thumbnail
  <Image source={{ uri: item.largeUrl }} style={{ width: 50, height: 50 }} />
);
```

### Good Example
```tsx
import { Image } from 'expo-image';

const ListItem = ({ item }) => (
  <Image
    source={item.thumbnailUrl} // Use resized thumbnails from CDN
    placeholder={item.blurHash}
    contentFit="cover"
    transition={200}
    style={{ width: 50, height: 50 }}
  />
);
```

### Notes
Always use a CDN to serve images at the rendered size. On Android, `expo-image` uses Glide which is significantly faster than React Native's default bridge.
