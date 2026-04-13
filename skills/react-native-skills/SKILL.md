---
name: react-native-skills
description: React Native best practices for AI agents. Use when building React Native or Expo apps, optimizing list performance, implementing animations with Reanimated, structuring monorepo projects, or managing application state.
---

# React Native & Expo Performance Skills

Comprehensive best practices for React Native and Expo applications.
**11 core rules across Performance, UI, Animation, Navigation, and State Management.**

## When to Apply

Reference these guidelines when:
- Building React Native or Expo apps
- Optimizing list and scroll performance
- Implementing animations with Reanimated
- Structuring monorepo projects
- Managing application state

## Rules by Priority

### CRITICAL (App Breaking)
Violating these causes severe performance degradation or crashes.

| Rule | Category | Impact |
|------|----------|--------|
| [Memoize List Items](rules/list-performance-item-memo.md) | Performance | Unmemoized lists freeze the app |
| [Use Virtualization Properly](rules/list-performance-virtualize.md) | Performance | Rendering 1000 items kills the device |
| [Minimize State Usage](rules/react-state-minimize.md) | State | Excessive state causes cascading re-renders |

### HIGH (Architecture)
Following these saves 40%+ rework and prevents subtle bugs.

| Rule | Category | Impact |
|------|----------|--------|
| [Avoid Inline Objects in Lists](rules/list-performance-inline-objects.md) | Performance | Inline styles cause re-renders |
| [Avoid Inline Functions in Lists](rules/list-performance-callbacks.md) | Performance | Breaks callback stability |
| [Use Derived Values for Animations](rules/animation-derived-value.md) | Animation | JS thread animations lag |
| [Use Native Navigators Properly](rules/navigation-native-navigators.md) | Navigation | Wrong navigator = startup delay |
| [Single Source of Truth](rules/state-ground-truth.md) | State | State duplication = sync bugs |
| [Keep Single Dependency Versions](rules/monorepo-single-dependency-versions.md) | Architecture | Version mismatch = long debugging |

### MEDIUM (Code Quality)
Following these prevents debugging friction and improves maintainability.

| Rule | Category | Impact |
|------|----------|--------|
| [Use Pressable for Interactions](rules/ui-pressable.md) | UI | Legacy touchables are hard to customize |
| [Use Scalable Styling Patterns](rules/ui-styling.md) | UI | Unstructured styles cause maintenance pain |

## Quick Reference

### Performance (4 Rules)
- `list-performance-item-memo`: Wrap list items in React.memo()
- `list-performance-virtualize`: Configure windowSize and initialNumToRender
- `list-performance-inline-objects`: Move inline styles to StyleSheet
- `list-performance-callbacks`: Use useCallback for render functions

### State (2 Rules)
- `react-state-minimize`: Reduce local state, use refs when possible
- `state-ground-truth`: One source of truth per piece of data

### UI (2 Rules)
- `ui-pressable`: Pressable instead of TouchableOpacity
- `ui-styling`: StyleSheet.create or Nativewind for consistency

### Animation (1 Rule)
- `animation-derived-value`: Use useDerivedValue for computed animations

### Navigation (1 Rule)
- `navigation-native-navigators`: Native Stack > JavaScript navigators

### Architecture (1 Rule)
- `monorepo-single-dependency-versions`: Strict version parity across packages

## How to Use

1. **New project?** Start with CRITICAL rules only.
2. **Existing codebase?** Adopt HIGH rules first.
3. **Optimization phase?** Add MEDIUM rules once core are solid.

Each rule has a detailed .md file with code examples and explanations.
