# Use Scalable Styling Patterns

## What It Is
Using structured styling patterns such as `StyleSheet.create` or specialized libraries (like Nativewind or Restyle) instead of ad-hoc, inline style objects.

## Why It Matters
Inline styles are difficult to maintain across large codebases, don't support responsive design themes well, and can cause performance issues because they generate new objects every render. Structured styles promote reusability and ensure a consistent "look and feel" across the entire application.

## The Wrong Way

```jsx
// Ad-hoc inline styling
const MyComponent = (props) => {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text style={{ 
        fontSize: 20, 
        fontWeight: 'bold', 
        color: props.isDark ? 'white' : 'black' 
      }}>
        Hello World
      </Text>
    </View>
  );
};
```

**What's wrong here:**
Styling logic (like theme switching) is mixed directly into the render function. This makes the code harder to read, prevents style reuse, and forces new object allocations on every render cycle.

## The Right Way

```jsx
const useStyles = (isDark) => StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: isDark ? '#1a1a1a' : '#ffffff',
  },
  title: {
    fontSize: 20,
    fontWeight: 'bold',
    color: isDark ? '#ffffff' : '#000000',
  },
});

const MyComponent = ({ isDark }) => {
  const styles = useStyles(isDark);
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Hello World</Text>
    </View>
  );
};
```

**What changed:**
1. Moved styling to a structured `StyleSheet.create` pattern.
2. Encapsulated theme-specific logic in a reusable `useStyles` hook or factory function.
3. Separation of concerns: the render function focuses on structure, while the styles handle appearance.

## Additional Context
For large-scale, professional applications, consider adopting `Nativewind` for utility-first styling or `Restyle` for a theme-based, type-safe design system. These tools further improve scalability beyond standard `StyleSheet`.

## Related Rules
- `list-performance-inline-objects`: Focuses on the performance impact of avoiding inline styles.
- `ui-pressable`: Leverages structured styles for interaction states.
