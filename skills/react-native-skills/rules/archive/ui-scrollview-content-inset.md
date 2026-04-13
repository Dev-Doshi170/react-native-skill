---
id: ui-scrollview-content-inset
title: ScrollView Content Inset
priority: medium
triggers:
  - scrollview padding bottom
  - items cut off in scrollview
  - scrollview contentContainerStyle
bad_patterns:
  - applying padding directly to ScrollView
  - using margins to fix scroll cut-off
platforms:
  - ios
  - android
  - cross-platform
category: ui
---

### Problem
Applying padding directly to a `ScrollView` or `FlatList` component usually fails to provide extra space at the end of the scrollable area.

### Why it matters
Users cannot scroll past the last item comfortably, or the last item is partially hidden behind bottom tabs.

### Bad Example
```tsx
<ScrollView style={{ paddingBottom: 50 }}> // Does NOT fix scroll end
  {content}
</ScrollView>
```

### Good Example
```tsx
<ScrollView contentContainerStyle={{ paddingBottom: 50 }}> // CORRECT
  {content}
</ScrollView>
```
