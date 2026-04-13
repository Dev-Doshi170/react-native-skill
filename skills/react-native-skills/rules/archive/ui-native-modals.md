---
id: ui-native-modals
title: Native Modal vs JS Overlays
priority: medium
triggers:
  - react native modal
  - bottom sheet crash
  - modal z-index
bad_patterns:
  - nested JS overlays that overlap incorrectly
  - ignoring hardware back button on Android
platforms:
  - ios
  - android
  - cross-platform
category: ui
---

### Problem
Using simple JS-based overlays for complex navigation or inputs can lead to z-index issues and poor platform integration (e.g., status bar not changing).

### Why it matters
Native modals handle the hardware back button, keyboard offsets, and screen-wide overlays more reliably than JS-rendered Views.

### Bad Example
```tsx
{isVisible && (
  <View style={StyleSheet.absoluteFill}> // JS Overlay
     <MyContent />
  </View>
)}
```

### Good Example
```tsx
<Modal
  visible={isVisible}
  animationType="slide"
  transparent={true}
  onRequestClose={handleClose} // Vital for Android
>
  <View style={styles.modalContent}>
    <MyContent />
  </View>
</Modal>
```

### Notes
For complex high-quality bottom sheets, prefer `@gorhom/bottom-sheet` which is optimized for performance.
