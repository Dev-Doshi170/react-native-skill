---
id: list-performance-function-references
title: Stable List Component References
priority: high
triggers:
  - flatlist blank screen
  - list re-mounting
  - unstable renderItem
bad_patterns:
  - defining renderItem inside the component body
  - anonymous functions in FlatList props
platforms:
  - ios
  - android
  - cross-platform
category: performance
---

### Problem
Defining `renderItem`, `ListHeaderComponent`, or `keyExtractor` as anonymous functions inside the render body causes React to see them as "new" functions on every render.

### Why it matters
This causes the entire list to unmount and remount every time the parent re-renders, leading to massive performance hits, loss of scroll position, and flickering.

### Bad Example
```tsx
const MyScreen = () => {
  return (
    <FlatList
      data={data}
      renderItem={({ item }) => <Text>{item.label}</Text>} // Bad: New ref every render
    />
  );
};
```

### Good Example
```tsx
// 1. Define outside the component if possible
const renderItem = ({ item }) => <ListItem item={item} />;

const MyScreen = () => {
  // 2. Or use useCallback for stable references
  const keyExtractor = useCallback((item) => item.id, []);
  
  return <FlatList data={data} renderItem={renderItem} keyExtractor={keyExtractor} />;
};
```

### Notes
Defining outside the component is best as it completely avoids any re-definition overhead and keeps the render body clean.
