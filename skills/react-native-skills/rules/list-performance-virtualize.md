# Use Virtualization Properly

## What It Is
Using virtualization properties like `windowSize`, `initialNumToRender`, and `removeClippedSubviews` to control how many items are kept in memory and rendered at once.

## Why It Matters
By default, FlatList might render too many items or keep a large buffer in memory. For lists with thousands of items, this can lead to "Out of Memory" crashes on low-end devices and significant initial loading lag.

## The Wrong Way

```jsx
// Default settings - might render too many items at once
<FlatList
  data={largeDataset}
  renderItem={renderItem}
  // No virtualization optimization
/>
```

**What's wrong here:**
With large datasets, the default `windowSize` (21) renders many screens worth of content, consuming excessive memory. Lack of `getItemLayout` forces the list to measure every item dynamically, causing "jumps" during fast scrolls.

## The Right Way

```jsx
// Optimized virtualization settings
<FlatList
  data={largeDataset}
  renderItem={renderItem}
  initialNumToRender={10}
  windowSize={5} // Renders 2 screens above/below instead of 10
  maxToRenderPerBatch={5}
  updateCellsBatchingPeriod={30}
  removeClippedSubviews={Platform.OS === 'android'} // Significant boost on Android
  getItemLayout={(data, index) => (
    {length: ITEM_HEIGHT, offset: ITEM_HEIGHT * index, index}
  )}
/>
```

**What changed:**
1. `windowSize` reduced to 5 screens instead of the default 21, significantly saving memory.
2. `removeClippedSubviews` enabled for Android to unmount off-screen views.
3. `getItemLayout` provided to skip dynamic measurement, making scrolling perfectly smooth.
4. `initialNumToRender` tuned to fill the initial screen without over-rendering.

## Additional Context
`getItemLayout` is the single biggest optimization if your items have a fixed height. For dynamic heights, try to estimate as accurately as possible or stick to virtualization defaults but reduce `windowSize`.

## Related Rules
- `list-performance-item-memo`: Essential to prevent re-renders within the virtualized window.
- `react-state-minimize`: Prevents state updates from flushing the virtualized cache too often.
