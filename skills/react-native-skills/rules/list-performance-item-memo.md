# Memoize List Items

## What It Is
Wrapping list items in `React.memo` to prevent unnecessary re-renders when the parent component re-renders but the item's props haven't changed.

## Why It Matters
In large lists, every state update in the parent (like scrolling position tracking or modal visibility) triggers a re-render of ALL items. This kills FPS from 60 to below 15, causing noticeable stuttering and lag.

## The Wrong Way

```jsx
// Unmemoized item - re-renders on every parent state change
const ListItem = ({ item, onPress }) => {
  console.log('Rendering item:', item.id);
  return (
    <Pressable style={styles.item} onPress={() => onPress(item.id)}>
      <Text>{item.title}</Text>
    </Pressable>
  );
};

const MyList = ({ data }) => {
  const [selectedId, setSelectedId] = useState(null);
  return (
    <FlatList
      data={data}
      renderItem={({ item }) => (
        <ListItem item={item} onPress={setSelectedId} />
      )}
      keyExtractor={item => item.id}
    />
  );
};
```

**What's wrong here:**
`ListItem` is a functional component that re-renders every time `MyList` re-renders (e.g., when `selectedId` changes), even for items that aren't selected and whose data hasn't changed.

## The Right Way

```jsx
// Memoized item with custom comparison
const ListItem = React.memo(({ item, onPress }) => {
  console.log('Rendering item:', item.id);
  return (
    <Pressable style={styles.item} onPress={() => onPress(item.id)}>
      <Text>{item.title}</Text>
    </Pressable>
  );
}, (prev, next) => prev.item.id === next.item.id && prev.onPress === next.onPress);

const MyList = ({ data }) => {
  const [selectedId, setSelectedId] = useState(null);
  const handlePress = useCallback((id) => setSelectedId(id), []);

  return (
    <FlatList
      data={data}
      renderItem={({ item }) => (
        <ListItem item={item} onPress={handlePress} />
      )}
      keyExtractor={item => item.id}
    />
  );
};
```

**What changed:**
1. `ListItem` wrapped in `React.memo` to enable shallow prop comparison.
2. `handlePress` stabilized with `useCallback` to prevent the `onPress` prop reference from changing and breaking memoization.
3. Optional custom comparison function added for extra safety when dealing with complex objects.

## Additional Context
Use this for any list with more than 10-15 items or items with complex layouts/logic. It is the single most effective way to maintain 60FPS scrolling in React Native.

## Related Rules
- `list-performance-virtualize`: Works together to manage memory and render counts.
- `list-performance-callbacks`: Essential for keeping `onPress` references stable.
