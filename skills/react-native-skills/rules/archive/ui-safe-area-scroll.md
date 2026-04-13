---
id: ui-safe-area-scroll
title: Safe Area Aware Scrolling
priority: high
triggers:
  - scrollview notch overlap
  - flatlist safe area
  - handle home indicator
bad_patterns:
  - wrapping ScrollView in SafeAreaView directly
  - ignoring bottom notches on modern devices
platforms:
  - ios
  - android
  - cross-platform
category: ui
---

### Problem
Wrapping a scrollable component in `SafeAreaView` clips the scrollable content to the safe zone, preventing the "flowing under the notch" effect.

### Why it matters
Creates a poor "web-like" UX where content abruptly stops at the safe area boundaries.

### Bad Example
```tsx
<SafeAreaView style={{ flex: 1 }}>
  <ScrollView>...</ScrollView> // Clips content at top/bottom
</SafeAreaView>
```

### Good Example
```tsx
import { useSafeAreaInsets } from 'react-native-safe-area-context';

const MyScreen = () => {
  const insets = useSafeAreaInsets();
  return (
    <ScrollView 
      contentContainerStyle={{ 
        paddingTop: insets.top, 
        paddingBottom: insets.bottom 
      }}
    >
      ...
    </ScrollView>
  );
};
```
