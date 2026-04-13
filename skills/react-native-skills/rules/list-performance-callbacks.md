# Avoid Inline Functions in Lists

## What It Is
Using `useCallback` or static functions for `renderItem`, `onPress`, and other list callbacks instead of defined arrow functions inside the render body.

## Why It Matters
Inline arrow functions `() => ...` break reference equality on every render. This prevents `React.memo` from working on children and can cause the `FlatList` itself to re-initialize its internal state or re-calculate layouts unnecessarily.

## The Wrong Way

```jsx
// Inline function causes re-renders
const MyList = ({ data, onItemPress }) => {
  return (
    <FlatList
      data={data}
      renderItem={({ item }) => (
        <ListItem 
          item={item} 
          onPress={() => onItemPress(item.id)} // NEW FUNCTION EVERY RENDER
        />
      )}
      keyExtractor={(item) => item.id.toString()} // NEW FUNCTION EVERY RENDER
    />
  );
};
```

**What's wrong here:**
The `onPress` and `keyExtractor` functions are re-created every time `MyList` re-renders. This constantly breaks the memoization of every `ListItem`, even if the data hasn't changed.

## The Right Way

```jsx
// Stable renderItem and callbacks
const MyList = ({ data, onItemPress }) => {
  
  const handlePress = useCallback((id) => {
    onItemPress(id);
  }, [onItemPress]);

  const renderItem = useCallback(({ item }) => (
    <ListItem item={item} onPress={handlePress} />
  ), [handlePress]);

  const keyExtractor = useCallback((item) => item.id.toString(), []);

  return (
    <FlatList
      data={data}
      renderItem={renderItem}
      keyExtractor={keyExtractor}
    />
  );
};
```

**What changed:**
1. `handlePress` and `renderItem` wrapped in `useCallback` to maintain stable references across renders.
2. `keyExtractor` stabilized, which helps FlatList track items efficiently during data updates.
3. Memoized `ListItem` (from the memo rule) will now successfully skip re-renders.

## Additional Context
For maximum performance, define `keyExtractor` outside the component scope if it only depends on the item's structure. If your `renderItem` doesn't use any local state, it can also be defined outside the component.

## Related Rules
- `list-performance-item-memo`: Is ineffective without stable callback references.
- `list-performance-inline-objects`: Another rule focused on maintaining reference stability.
