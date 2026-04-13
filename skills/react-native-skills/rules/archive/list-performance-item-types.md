---
id: list-performance-item-types
title: Multiple List Item Types
priority: medium
triggers:
  - flatlist multiple item types
  - mixed density list
  - performance several item types
bad_patterns:
  - single renderItem with complex conditionals
  - failing to use getItemType
platforms:
  - ios
  - android
  - cross-platform
category: performance
---

### Problem
Rendering a single complex `renderItem` with many conditional branches for different UI types is inefficient for React's reconciliation.

### Why it matters
Increases the complexity of the "diffing" process and can lead to re-renders of the entire item even when only the type should change.

### Bad Example
```tsx
const renderItem = ({ item }) => {
  if (item.type === 'header') return <Header ... />;
  if (item.type === 'footer') return <Footer ... />;
  return <Item ... />;
};
```

### Good Example
```tsx
// Define separate memoized components for each type
const HeaderItem = React.memo(({ data }) => <Header ... />);
const RegularItem = React.memo(({ data }) => <Item ... />);

const renderItem = ({ item }) => {
  switch(item.type) {
    case 'header': return <HeaderItem data={item} />;
    default: return <RegularItem data={item} />;
  }
};
```
