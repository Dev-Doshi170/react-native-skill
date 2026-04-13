---
id: rendering-text-in-text-component
title: Wrap All Text in Text Components
priority: high
triggers:
  - react native text crash
  - virtual text error
  - raw text nodes
bad_patterns:
  - rendering strings directly inside View
  - using console.log strings in UI
platforms:
  - ios
  - android
  - cross-platform
category: ui
---

### Problem
React Native does not support raw text nodes directly inside layout components like `View` or `ScrollView`. All text MUST be children of a `Text` component.

### Why it matters
React Native will throw a "Invariant Violation: Text strings must be rendered within a <Text> component" error, causing the app to crash.

### Bad Example
```tsx
<View>
  Hello World // CRASH
</View>
```

### Good Example
```tsx
<View>
  <Text>Hello World</Text> // CORRECT
</View>
```
