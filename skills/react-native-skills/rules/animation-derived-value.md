# Use Derived Values for Animations

## What It Is
Using Reanimated's `useDerivedValue` hook to perform calculations that depend on other shared values directly on the UI thread.

## Why It Matters
Performing animation-related logic in JavaScript and then syncing it back to shared values incurs a cost in crossing the "bridge" between the JS and UI threads. `useDerivedValue` ensures these calculations happen entirely on the UI thread, maintaining a solid 60 FPS even during complex, interdependent animations.

## The Wrong Way

```jsx
// Calculating derived state in JS thread
const offset = useSharedValue(0);
const [scaledValue, setScaledValue] = useState(0);

// WRONG: Updating state or performing logic in JS during animation
useEffect(() => {
  // Trying to calculate scale in JS based on offset... LAGGING
  const newScale = offset.value / 100;
  setScaledValue(newScale);
}, [offset.value]);

const animatedStyle = useAnimatedStyle(() => {
  return { 
    transform: [
      { translateX: offset.value },
      { scale: scaledValue } // THIS WILL JITTER
    ] 
  };
});
```

**What's wrong here:**
Syncing animation values to React state or performing heavy math in the JS thread during an animation causes jitter and frame drops because the UI thread must wait for the JS thread to respond, which is often busy with other tasks.

## The Right Way

```jsx
import { useSharedValue, useDerivedValue, useAnimatedStyle, interpolate } from 'react-native-reanimated';

const offset = useSharedValue(0);

// Derived value stays entirely on the UI thread
const scale = useDerivedValue(() => {
  // High-performance interpolation on the UI thread
  return interpolate(offset.value, [0, 100], [1, 2]);
});

const animatedStyle = useAnimatedStyle(() => {
  return {
    transform: [
      { translateX: offset.value },
      { scale: scale.value }
    ],
  };
});
```

**What changed:**
1. Used `useDerivedValue` for all dependent animation calculations.
2. The entire animation, including all dependencies, now runs exclusively on the UI thread.
3. Eliminated the JS-UI bridge overhead for this animation loop, ensuring fluid performance.

## Additional Context
`useDerivedValue` is essentially the `useMemo` of the Reanimated world. Use it whenever one animation value depends on another, or when you need to perform complex math during a frame update.

## Related Rules
- `ui-pressable`: Pressable interactions often trigger these optimized animations.
- `list-performance-virtualize`: Smooth animations are especially critical when scrolling through virtualized lists.
