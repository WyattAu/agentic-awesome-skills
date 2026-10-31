(`react-native-keyboard-controller` only for the keyboard recipe.) `expo install`, not `npm install`; it resolves the versions that match the SDK. The worklets Babel plugin is configured by `babel-preset-expo` automatically.
`GestureHandlerRootView` wraps the app once; in Expo Router, the root `_layout`:
**Gesture Handler v3:** Expo installs v2, and the recipes use its `Gesture.Pan()` builder. If the project is already on v3, the builder is legacy; each gesture is a hook taking one config object, with `onStart` → `onActivate`, `onEnd` → `onDeactivate`, and the `success` flag replaced by `event.canceled` (inverted). The hook manages its own identity, so drop the `useMemo`:
// Apple's exponential-decay form; not the v²/2a from physics class.
Every pressable in the app. This passes the frequency gate only because it's near-imperceptible: 120ms and a 3% scale is the ceiling for something touched this often; anything longer or larger belongs to rarer moments, per step 1 in SKILL.md. No gesture, no shared value; a CSS transition is the whole implementation.
`setState` is fine here; it fires twice per press, not per frame. `hitSlop` brings a small icon up to the 44pt target without growing it; `pressRetentionOffset` stops a slight finger drift from cancelling.
- **`onStart` captures the current value.** Without it, grabbing a sheet mid-animation teleports it; the animation must continue from where the eye last saw it.
- **`overshootClamping` on dismissal**; otherwise the sheet springs past the bottom of the screen and flashes a gap.
Before writing this: gesture-handler ships [`ReanimatedSwipeable`](https://docs.swmansion.com/react-native-gesture-handler/docs/components/reanimated_swipeable/), which already does swipe-to-reveal actions; thresholds, overshoot, open/close methods; on the UI thread. Reach for it when the row reveals action buttons. Build the gesture yourself only when the interaction is different in kind: swipe-to-commit with momentum projection, like this one.
const ROW_CLOSE = LinearTransition.duration(200);   // module scope; builders rebuilt in render cost every re-render
**Never animate the header's `height` to collapse it.** That runs a layout pass on the header and everything below it on every scroll frame; the one animation guaranteed to stutter, because it's competing with the scroll itself. Give the container a fixed height and translate the content inside it, clipping with `overflow: 'hidden'`.
// or in useMemo; an inline chain in JSX rebuilds the builder on every render.
**Never put `entering` on a row inside `FlatList`, `FlashList`, or any virtualized list.** Rows are recycled, so the animation re-fires every time one scrolls back into view; the list appears to flicker while the user scrolls. Animate the list container once on mount, or use `itemLayoutAnimation` for reflow only.
// Root _layout, next to GestureHandlerRootView; hooks below do nothing without it.
This is the sanctioned `width` animation: the pill is absolutely positioned with no children, so nothing else re-lays-out, and its corner radius survives; `scaleX` would smear the corners into ovals.
| Deeper into a hierarchy | `animation: 'default'`; the platform push, unmodified |
`formSheet` is native on both platforms, but not the same on both; the [Expo modal docs](https://docs.expo.dev/router/advanced/modals/#form-sheet-presentation) have the full list:
- **Android caps detents at three.** A longer `sheetAllowedDetents` array works on iOS and silently truncates on Android; design for three.
- **`fitToContents` needs explicitly sized content.** A `flex: 1` root has no intrinsic height to fit; size the content, or the detent is wrong.
// Module scope; layout-animation builders live outside the component.
- **The 300ms cap holds here too.** A toast isn't an exception; it's uninvited, so if anything it should be quicker and quieter than motion the user asked for.
If toasts stack and the list reflows, add `itemLayoutAnimation` and expect to tune the opacity against the reflow by eye; there's no formula for that pair. Look at it again the next day.
When a crossing point matters; a detent, a snap, a pull-to-refresh arming; don't poll it from JS and don't `scheduleOnRN` every frame.