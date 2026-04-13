# Use Native Navigators Properly

## What It Is
Preferring `@react-navigation/native-stack` over the legacy `@react-navigation/stack` for stack-based navigation.

## Why It Matters
Native stack uses the platform's native navigation primitives (`UINavigationController` on iOS and `Fragment` navigation on Android). This results in significantly better performance, smoother native transitions, better memory management, and a 2-3s faster app startup time compared to JavaScript-based stacks.

## The Wrong Way

```jsx
import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator(); // JS-based stack

function MyStack() {
  return (
    <Stack.Navigator>
      <Stack.Screen name="Home" component={Home} />
      <Stack.Screen name="Details" component={Details} />
    </Stack.Navigator>
  );
}
```

**What's wrong here:**
`createStackNavigator` is implemented entirely in JavaScript. While highly customizable, it mimics native behavior rather than using it, leading to potential frame drops during complex transitions and higher memory overhead.

## The Right Way

```jsx
import { createNativeStackNavigator } from '@react-navigation/native-stack';

const Stack = createNativeStackNavigator(); // Native-based stack

function MyStack() {
  return (
    <Stack.Navigator 
      screenOptions={{
        headerTitleStyle: { fontWeight: 'bold' },
        // Native stack is more performant and uses native headers
      }}
    >
      <Stack.Screen name="Home" component={Home} />
      <Stack.Screen name="Details" component={Details} />
    </Stack.Navigator>
  );
}
```

**What changed:**
1. Switched from `createStackNavigator` to `createNativeStackNavigator`.
2. Transitions and gestures are now handled entirely by the OS, ensuring perfect fluid performance.
3. System resources are managed more efficiently by the native platform.

## Additional Context
Always use Native Stack unless you require highly specific JavaScript-only transition animations that the native platform's navigation controllers cannot support. Native stack is the recommended default for all modern React Native apps.

## Related Rules
- `ui-native-modals`: Often used in conjunction with native navigation for a premium feel.
- `animation-derived-value`: While navigation is handled by the OS, intra-screen animations should remain performant.
