# GitHub Copilot Instructions

## React Native Performance & Architecture

When providing suggestions for this repository, prioritize the following patterns defined in `skills/react-native-skills/SKILL.md`:

- **Lists:** Always wrap `FlatList` items in `React.memo` and move styles out of the render function.
- **Animations:** Use `useDerivedValue` and worklets for Reanimated logic.
- **Interactions:** Use the `Pressable` API instead of `TouchableOpacity`.
- **State:** Minimize local state to prevent unnecessary re-renders.
- **Navigation:** Prefer Native Stack navigators for better performance.

## Reference
Guidelines can be found at: `skills/react-native-skills/SKILL.md`
