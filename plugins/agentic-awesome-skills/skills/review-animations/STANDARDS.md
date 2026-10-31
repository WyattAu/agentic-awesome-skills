**Never animate keyboard-initiated actions**; they repeat hundreds of times daily; animation makes them feel slow and disconnected. (Raycast has no open/close animation; correct for something used hundreds of times a day.)
Find curves at [easing.dev](https://easing.dev/) or [easings.co](https://easings.co/); don't hand-roll from scratch.
  **Modals are exempt**; they appear centered in the viewport, keep `transform-origin: center`.
Feel natural because they simulate physics; no fixed duration; they settle on parameters. Use for: drag with momentum, "alive" elements (Dynamic Island), interruptible gestures, decorative mouse-tracking.
// Apple-style (easier to reason about); recommended
Keep bounce subtle (0.1–0.3); avoid bounce in most UI; reserve for drag-to-dismiss and playful interactions. Springs maintain velocity when interrupted (keyframes restart from zero), so they're ideal for gestures users may reverse mid-motion.
/* Interruptible; good for dynamic UI */
/* Not interruptible; avoid for dynamic UI */
- **Prefer `transform` and `opacity`**; often compositor-friendly; verify actual layer/rasterization behavior. `padding`/`margin`/`height`/`width`/`top`/`left` trigger all three rendering steps.
- **Don't drive child transforms via a CSS variable on the parent**; it recalcs styles for all children. Set `transform` directly on the element.
- **CSS animations beat JS under load**; they run off the main thread; rAF-based animations stutter while the browser loads/scripts/paints. Use CSS for predetermined motion, JS for dynamic/interruptible.
- **`translate` percentages** are relative to the element's own size; `translateY(100%)` moves by the element's height regardless of dimensions (how Sonner/Vaul position toasts/drawers). Prefer over hardcoded px.
- **`scale()` scales children too** (font, icons, content); a feature for press feedback.
- **Momentum dismissal**: don't require crossing a distance threshold; compute velocity (`Math.abs(distance)/elapsedMs`); dismiss if `> ~0.11`. A flick should be enough.
- **Multi-touch protection**: ignore extra touch points after the drag begins (`if (isDragging) return`); prevents jumps.
- **Friction over hard stops**; allow over-drag with rising resistance rather than an invisible wall.
Stagger group entrances; 30–80ms between items. Longer delays feel slow. Stagger is decorative; never block interaction while it plays.
  .element:hover { transform: scale(1.05); } /* gate hover motion; touch fires false hovers on tap */
- **Real devices** for gestures (drawers, swipe); connect a phone, hit the dev server by IP, use Safari remote devtools.
- **Fresh eyes next day**; imperfections invisible during development surface later.
Match motion to the component's personality: playful can be bouncier; a professional dashboard should be crisp and fast. Sonner feels right partly because easing, duration, design, and even the name are in harmony; slightly slower, `ease` rather than `ease-out`, to feel elegant. Opacity + height in entering/exiting lists is trial and error; there's no formula; adjust until it feels right.