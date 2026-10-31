A construction skill for React Native. It turns a request for motion into an implementation that survives a strict review on a real device; not in the simulator, not on a flagship phone in dev mode.
3. **The user's finger is on the element.** Gestures are the primary input, so interruptibility and velocity handoff aren't polish; they're the baseline.
2. **Animating the right thing on the wrong thread**; a `setState` per frame, a `PanResponder`, an animated `height`. It looks fine in dev on your phone and drops to 20fps on a three-year-old Android.
| 100+ times/day; tab switches, keyboard open/close, scrolling, toggles in settings | **No animation.** Platform default or nothing. Stop here. |
| Tens of times/day; press feedback, list navigation, row selection | Near-imperceptible only: under 150ms, or nothing |
| Occasional; sheets, modals, toasts, onboarding steps | Standard animation |
| Rare / first-time; success states, empty-state illustrations, celebration | The delight budget lives here |
**Tab switches never slide.** Tabs are peers, not a hierarchy; sliding implies depth that isn't there, and the user pays for it dozens of times a session. `animation: 'none'`.
### 3. Pick the tool: cheapest that works
| A state-driven change with no gesture; press, toggle, color, a value flipping | **Reanimated CSS transition** (`transitionProperty` in the style) |
| A bottom sheet that is its own screen | **`presentation: 'formSheet'`**; it's a real UISheetPresentationController, free and correct |
| Tab bar | **`NativeTabs`** (from `expo-router/unstable-native-tabs`); the platform's real tab bar, its behaviors and transitions included |
| Context menu, press-and-hold preview | **`Link.Menu` / `Link.Preview`** (Expo Router, iOS-only); native menus and peek, never rebuilt in JS |
| Header that collapses into a large title | **`headerLargeTitleEnabled`** on the native stack (iOS-only; `headerLargeTitle` is deprecated); not a scroll worklet |
| Pull to refresh | **`RefreshControl`**; hand-roll only when it's a signature interaction (see the threshold recipe) |
| UI that tracks the keyboard | **`react-native-keyboard-controller`**; the keyboard's real position, frame by frame, on the UI thread |
| Vector illustration, celebration, empty state | **Lottie**; for illustration only, never for UI state |
| A huge animated scene, freeform drawing | **`@shopify/react-native-skia`**; a canvas, for when the view hierarchy itself is the bottleneck |
**Dependencies.** Install with `npx expo install <package>`; it resolves the version that matches the project's SDK, which plain `npm install` won't:
| Keyboard-following UI | `react-native-keyboard-controller` (needs `KeyboardProvider` at the root; see the keyboard recipe) |
- **The one exception: an absolutely positioned element with no children**; a tab pill, a progress bar fill. It's out of flow, so nothing else re-lays-out, and animating `width` keeps the corner radius that `scaleX` would smear.
- **`transform` is an array and order matters**; `[{ translateY }, { scale }]` scales after moving; reversed, the translate gets scaled too. Keep translate first unless you want the multiplication.
- **Percentages work in `translate`** and are relative to the element's own size; `translateY('100%')` moves a sheet by its own height whatever its content.
Reanimated's spring takes Apple's two designer parameters directly; use this form, not mass/stiffness/damping:
**Never `ease-in` on UI.** It starts slow, delaying the exact moment the user is watching. Reanimated's built-ins are as weak as CSS's; use these:
| Screen transition | the platform default; don't override it |
- **Never schedule back to the RN runtime inside `onUpdate` or a scroll handler.** `scheduleOnRN(fn, ...args)` from `react-native-worklets`; the Reanimated 4 replacement for the deprecated `runOnJS(fn)(...args)`; queues an RN-runtime call, and in `onUpdate` that's 60–120× per second. It belongs in `onEnd`, or in a `useAnimatedReaction` that fires when a value crosses a threshold.
- **Never read a shared value during render** (`translateY.get()` in JSX). It's a snapshot that never updates and it silently desyncs. **Never write one during render either**; it fires mid-reconciliation, and a re-render you didn't cause replays the write. Touch shared values only in worklets, handlers, and effects.
- **Use `.get()` / `.set()`, not `.value`.** Same API, but direct `.value` access is the form the React Compiler can't see through; the Reanimated docs call `get`/`set` the compiler-safe way. `set` also takes a functional update: `sv.set((v) => v + 1)`.
- **Feedback on press-in, commit on press-out.** Waiting for the tap to complete before showing anything feels dead; this is the latency the user actually perceives.
- **`scale: 0.97` in 100–150ms** on any button-like pressable, `Pressable` + a CSS transition. `scale` takes the label and icons with it, which is what makes it read as physical. Full-width list rows are the exception: they highlight their background instead; a scaling row reads as the whole screen squishing.
- **44×44pt minimum touch target** (48dp Android). If the visual is smaller, add `hitSlop`; don't grow the visual.
| A value ticks past a step; picker, slider detent, segmented control | `Haptics.selectionAsync()` |
- **Same frame as the visual.** A haptic that lags its animation reads as a glitch, not as feedback. Fire it at the causal moment; the detent catching; not when the animation finishes.
**Text scales.** `allowFontScaling` is on by default, so any height you measured at default type size is wrong at 200%. Never animate to a hardcoded height; measure with `onLayout`, or animate a transform instead.
- Install through Expo so versions match the SDK: `npx expo install react-native-reanimated react-native-worklets`. In an Expo project, `babel-preset-expo` configures the worklets Babel plugin automatically; no `babel.config.js` step. Only a bare RN project without that preset adds the plugin manually, and there it must be last in the list. A missing or misplaced plugin doesn't silently fall back anymore; it throws `Failed to create a worklet` at runtime.
On ProMotion iPhones, third-party animations are capped at 60fps unless `CADisableMinimumFrameDurationOnPhone` is set. Recent Expo SDKs set it by default; confirm it's there, and add it if not:
For ready-to-build implementations; press feedback, drag-to-dismiss sheet, swipe-to-delete, collapsing header, list entrances, keyboard-synced UI, tab indicator, screen transitions; see [RECIPES.md](RECIPES.md). Load it whenever the request matches one; start from the recipe rather than from a blank file.
| Distance-only dismissal threshold | velocity **or** distance; a flick is enough |
- **The gate result**; frequency tier and named purpose. Say what you rejected and why.
- **The ingredients**; tool, properties, spring or curve + duration, thread.
- **What to feel-check on device**; gestures, velocity handoff and haptic timing cannot be judged from code. Name what to try: flick it, interrupt it mid-flight, reverse it, run it on the slowest Android you have.