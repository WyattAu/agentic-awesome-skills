State variables-both React `useState` and Reanimated shared values-should
1. **Single source of truth**; The state (`pressed`) describes what's
2. **Easier to extend**; Adding opacity, rotation, or other effects just
3. **Debugging**; Inspecting `pressed = 1` is clearer than `scale = 0.95`
4. **Reusable logic**; The same `pressed` value can drive multiple visual