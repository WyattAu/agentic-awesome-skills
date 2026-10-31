Curated visual styles for Frontend Slides. Each preset is inspired by real design references; no generic "AI slop" aesthetics. **Abstract shapes only; no illustrations.**
**Layout:** Split panel-white top, blue bottom. Brand marks in corners.
**Layout:** Split panels-electric blue left, dark right. Script accents.
- Display: `Cormorant` (400/600); elegant serif
- **No illustrations-only abstract CSS shapes**
- Display: `Bodoni Moda` (400/700); classic editorial
- Display: `Fraunces` (700/900); distinctive serif
- **No illustrations-only geometric CSS shapes**
**WRONG; silently ignored by browsers (no console error):**
**CORRECT; wrap in `calc()`:**
CSS does not allow a leading `-` before function names. The browser silently discards the entire declaration; no error, the element just appears in the wrong position. **Always use `calc(-1 * ...)` to negate CSS function values.**