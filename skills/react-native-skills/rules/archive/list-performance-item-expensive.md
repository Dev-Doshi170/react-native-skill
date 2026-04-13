---
id: list-performance-item-expensive
title: Avoiding Expensive Logic in List Items
priority: high
triggers:
  - heavy list item
  - slow list rendering
  - complex renderItem logic
bad_patterns:
  - complex calculations inside renderItem
  - data transformation inside item components
platforms:
  - ios
  - android
  - cross-platform
category: performance
---

### Problem
Performing heavy data processing, date formatting, or complex regex inside a `renderItem` component slows down every item's render cycle.

### Why it matters
The cumulative delay across dozens of visible items leads to noticeable lag and delayed response to user interactions.

### Bad Example
```tsx
const ListItem = ({ item }) => {
  // Expensive calculation on every render
  const formattedDate = heavyDateLibrary.format(item.timestamp, 'full');
  const complexValue = item.list.reduce((acc, val) => acc + val.price, 0);
  
  return <Text>{formattedDate}: {complexValue}</Text>;
};
```

### Good Example
```tsx
// 1. Pre-process data before passing to the list
const processedData = useMemo(() => data.map(item => ({
  ...item,
  formattedDate: format(item.timestamp),
  totalPrice: item.list.reduce((acc, v) => acc + v.price, 0)
})), [data]);

<FlatList data={processedData} ... />
```

### Notes
If pre-processing is not possible, use `useMemo` inside the item component, but pre-processing the entire dataset once is always faster.
