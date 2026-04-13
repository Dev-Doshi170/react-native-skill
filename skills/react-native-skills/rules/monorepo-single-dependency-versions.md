# Keep Single Dependency Versions

## What It Is
Enforcing a single version for shared core dependencies (like `react`, `react-native`, `expo`) across all packages and apps in a monorepo.

## Why It Matters
Version mismatches in a monorepo are a frequent source of "phantom" bugs. Metro bundler may accidentally load two different versions of React, which breaks Hooks. It can also cause duplicate native dependencies to be linked, leading to build-time crashes or obscure runtime failures that take hours to debug.

## The Wrong Way

```json
// apps/mobile/package.json
"dependencies": {
  "react-native": "0.72.3"
}

// packages/ui-library/package.json
"dependencies": {
  "react-native": "0.71.8" // CONFLICTING VERSION
}
```

**What's wrong here:**
Two different versions of `react-native` are requested. The package manager will install both into different `node_modules` folders. Metro might pick the wrong one during bundling, or the native build process might fail due to conflicting header files.

## The Right Way

```json
// root package.json (using Yarn/NPM workspaces)
{
  "workspaces": [
    "apps/*",
    "packages/*"
  ],
  "resolutions": {
    "react-native": "0.72.3",
    "react": "18.2.0"
  }
}

// packages/ui-library/package.json
{
  "peerDependencies": {
    "react-native": "*"
  }
}
```

**What changed:**
1. Use `resolutions` (in Yarn) or `overrides` (in NPM) in the root `package.json` to force specific versions across the entire workspace.
2. Shared libraries use `peerDependencies` instead of `dependencies` for core frameworks, ensuring they "borrow" the version installed by the main app.
3. Centralizing versions makes upgrading the entire project much safer and more predictable.

## Additional Context
Use automation tools like `syncpack` to keep all `package.json` files in sync. For Expo projects, `npx expo install --fix` can also help align dependencies to recommended versions within a workspace.

## Related Rules
- `navigation-native-navigators`: Often relies on specific versions of native dependencies.
- `animation-derived-value`: Performance depends on tight alignment between Reanimated and React Native versions.
