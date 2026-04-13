---
id: rendering-no-falsy-and
title: Prevent Falsy AND Rendering
priority: high
triggers:
  - react native crash 0
  - conditional rendering react native
  - zero showing in UI
bad_patterns:
  - {count && <Component />} (when count is 0)
  - {array.length && <List />} (when length is 0)
platforms:
  - ios
  - android
  - cross-platform
category: performance
---

### Problem
In React Native, using `{0 && <View />}` will render the number `0` as a text node. This causes a crash if not wrapped in `<Text>`, or an ugly "0" in the UI.

### Why it matters
Leads to unpredictable app crashes and poor UI quality. Unlike web, React Native is strict about text nodes.

### Bad Example
```tsx
const [cartItems, setCartItems] = useState([]);

return (
  <View>
    {/* If cartItems is [], it renders "0" and potentially crashes */}
    {cartItems.length && <Badge count={cartItems.length} />}
  </View>
);
```

### Good Example
```tsx
// 1. Explicit boolean comparison
{cartItems.length > 0 && <Badge count={cartItems.length} />}

// 2. Explicit cast
{!!cartItems.length && <Badge count={cartItems.length} />}

// 3. Ternary (Always safe)
{cartItems.length ? <Badge count={cartItems.length} /> : null}
```

### Notes
Ternary `condition ? element : null` is the safest and most readable pattern in React Native.
