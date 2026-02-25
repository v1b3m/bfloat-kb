# Frontend Bug: CSSStyleDeclaration Indexed Property Setter on Web

**Date:** 2026-02-23
**Project:** Expo 54 + React Native Web + NativeWind todo app
**Platform affected:** Web only (iOS/Android unaffected)

## Error

```
Uncaught Error
Failed to set an indexed property [0] on 'CSSStyleDeclaration': Indexed property setter is not supported.
```

Stack trace pointed to `setValueForStyle` in `react-dom`, occurring during `setInitialProperties` → `completeWork` — i.e., during the initial render of a DOM element.

The final clue was in the error output: **"The above error occurred in the `<a>` component."**

## Root Cause

**`Link` with `asChild` from expo-router** renders a native `<a>` tag on web. When styles are passed to the child `Pressable`, they get forwarded to the `<a>` element. React Native-only style properties that use **object values** (not flat strings/numbers) crash React DOM's style setter.

The specific offender:

```ts
shadowOffset: { width: 0, height: 6 }
```

When React DOM iterates over this object to set CSS properties, it tries to do `style[0] = ...` on the CSSStyleDeclaration, which is not allowed — CSSStyleDeclaration only supports named property setters, not indexed ones.

Other RN-only shadow properties (`shadowColor`, `shadowOpacity`, `shadowRadius`) and `elevation` don't crash because they're flat values that simply get ignored as unknown CSS properties. But `shadowOffset` is an **object**, and that's what causes the indexed property error.

## Resolution

Two changes were required:

### 1. Replace `Link asChild` with `router.push()`

Before (crashes on web):
```tsx
<Link href="/modal" asChild>
  <Pressable style={[styles.fab, { backgroundColor: ACCENT }]}>
    <IconSymbol name="plus" size={28} color="#fff" />
  </Pressable>
</Link>
```

After (works everywhere):
```tsx
<Pressable
  style={[styles.fab, { backgroundColor: ACCENT }]}
  onPress={() => router.push("/modal")}
>
  <IconSymbol name="plus" size={28} color="#fff" />
</Pressable>
```

### 2. Remove `shadowOffset` (and related iOS-only shadow props) from StyleSheet

Before:
```ts
fab: {
  // ...layout styles...
  shadowColor: "#FF8C42",
  shadowOffset: { width: 0, height: 6 },  // <-- crashes web
  shadowOpacity: 0.35,
  shadowRadius: 12,
  elevation: 8,
}
```

After:
```ts
fab: {
  // ...layout styles...
  elevation: 8,
}
```

## Lessons Learned

1. **`Link asChild` on web passes all child styles to a raw `<a>` DOM element.** Any React Native-only style with an object value (like `shadowOffset`, `transform` arrays from Reanimated) will crash `react-dom`'s style setter.

2. **`Animated.createAnimatedComponent(Pressable)` is also unsafe on web.** Reanimated's `useAnimatedStyle` returning `transform: [{ scale: value }]` can trigger the same indexed property error. Use `Animated.View` wrapping a plain `Pressable` instead.

3. **Prefer `router.push()` over `Link asChild`** when the child has complex RN styles (shadows, transforms, elevation). `Link` without `asChild` is fine since it controls its own rendering. `Link asChild` is fine when styles are web-safe (no object-valued properties).

4. **For cross-platform shadows**, use `elevation` (Android) + `boxShadow` (web) via Platform.select, or use a library like `react-native-shadow-2`. The `shadow*` properties are iOS-only.

## Quick Reference: Styles That Break on Web via `<a>`

| Property | Value Type | Breaks Web? |
|---|---|---|
| `shadowOffset` | `{ width, height }` | Yes |
| `transform` (RN format) | `[{ scale: 1 }]` | Yes (via Reanimated) |
| `shadowColor` | `string` | No (ignored) |
| `shadowOpacity` | `number` | No (ignored) |
| `shadowRadius` | `number` | No (ignored) |
| `elevation` | `number` | No (ignored) |
