# Avoid Inline Objects in Lists

## What It Is
Moving style objects and other static configurations out of the `renderItem` or component body to prevent reference changes on every render.

## Why It Matters
Inline objects `{}` are created fresh every time the parent component re-renders. If passed to a memoized component, they break `React.memo` because `{a:1} !== {a:1}` in terms of reference. This causes hidden re-renders that are difficult to debug.

## The Wrong Way

```jsx
// Inline style object breaks memoization
<FlatList
  data={data}
  renderItem={({ item }) => (
    <ListItem 
      item={item} 
      style={{ backgroundColor: 'white', padding: 10 }} // NEW OBJECT EVERY RENDER
    />
  )}
/>
```

**What's wrong here:**
The `style` prop receives a new object reference every time the component containing this `FlatList` renders. This forces `ListItem` to re-render even if it is wrapped in `React.memo`, negating any performance gains.

## The Right Way

```jsx
const styles = StyleSheet.create({
  itemContainer: {
    backgroundColor: 'white',
    padding: 10,
  },
});

// Static style reference
const MyList = ({ data }) => {
  return (
    <FlatList
      data={data}
      renderItem={({ item }) => (
        <ListItem 
          item={item} 
          style={styles.itemContainer} // STABLE REFERENCE
        />
      )}
    />
  );
};
```

**What changed:**
1. Moved styles to a `StyleSheet.create` outside the component scope.
2. The `style` prop now receives a stable numeric handle (in production) or stable object reference, preserving memoization.

## Additional Context
This applies to all props that accept objects or arrays: `initialParams`, `contentContainerStyle`, `columnWrapperStyle`, and even data arrays if they are being filtered/mapped inline.

## Related Rules
- `list-performance-item-memo`: Is rendering useless if inline objects break it.
- `ui-styling`: Encourages centralized styles for both performance and maintainability.
