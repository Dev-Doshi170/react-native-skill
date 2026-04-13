# Use Pressable for Interactions

## What It Is
Using the `Pressable` component instead of legacy components like `TouchableOpacity` or `TouchableHighlight` for more precise control over interaction states.

## Why It Matters
Legacy touchables are difficult to customize (e.g., changing text color when pressed) and have inconsistent behavior across platforms. `Pressable` is the modern, flexible standard for all touch interactions in React Native, providing high-fidelity feedback and better accessibility support.

## The Wrong Way

```jsx
// Legacy TouchableOpacity
<TouchableOpacity 
  style={styles.button} 
  onPress={handlePress}
  activeOpacity={0.7}
>
  <Text style={styles.buttonText}>Submit</Text>
</TouchableOpacity>
```

**What's wrong here:**
The styling options are limited to opacity changes on the container. It is difficult to implement complex press animations, such as changing the color or size of child elements (like the text or an icon) based on the press state.

## The Right Way

```jsx
<Pressable
  onPress={handlePress}
  style={({ pressed }) => [
    styles.button,
    { 
      backgroundColor: pressed ? '#005bb5' : '#007aff',
      transform: [{ scale: pressed ? 0.98 : 1 }]
    }
  ]}
>
  {({ pressed }) => (
    <Text style={[
      styles.buttonText,
      { color: pressed ? '#e1e1e1' : 'white' }
    ]}>
      Submit
    </Text>
  )}
</Pressable>
```

**What changed:**
1. Switched from `TouchableOpacity` to `Pressable`.
2. Used the function-as-style pattern to reactively change background color and scale based on the `pressed` state.
3. Used the function-as-children pattern to change the text color, providing a more premium, integrated feedback feel.
4. Gained more precise control without relying on global properties like `activeOpacity`.

## Additional Context
`Pressable` also provides a range of granular callbacks including `onPressIn`, `onPressOut`, `onLongPress`, and `delayLongPress`. It is the building block for all modern design systems in React Native.

## Related Rules
- `ui-styling`: Works with Pressable to maintain consistent interactive styles.
- `animation-derived-value`: Can be used for more complex, high-performance press animations.
