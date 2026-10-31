- In SDK 56+, never import from `@react-navigation/*` directly; use `expo-router/react-navigation` instead (covers `@react-navigation/native`, `/core`, `/elements`, `/routers`)
- Don't pass `Color` / `PlatformColor` values into Reanimated styles; use static colors there (see `references/animations.md`).
  _layout.tsx; <NativeTabs />
    _layout.tsx; <Stack />
    index.tsx; Main list
    search.tsx; Search view