### 8. Coherence (VISUAL-CRAFT.md §C0: the "one choice per axis" laws)
> The biggest reason a UI reads as "AI-generated" isn't ugly parts; it's *mixed*
- [ ] **One radius personality**; sharp (0-4px) OR soft (8-12px) OR pill, applied to every card/button/input/modal. No mixing (e.g. a `rounded-none` panel with `rounded-full` buttons).
- [ ] **One accent color** for interactive emphasis (+ semantic red/green/amber only); not two+ competing accents.
- [ ] **No emoji as UI icons** (🚗🧺⭐ as list/nav/status/category markers); they inject many uncontrolled hues; use one line-icon set in `currentColor`.
- [ ] **Status color = severity, not decoration**; a normal/OK/"보통" state is neutral grey (not colored); color marks only the minority of rows that need attention; same value → same color.
- [ ] **No decorative hues**; favorite stars, category dots, avatars use the accent or grey, not a new color each.
- [ ] **One shadow language**; same light direction, same scale/tint; not some black + some tinted, some up-lit + some down-lit.
- [ ] **Nested-radius law**; an element inside a rounded container uses `inner = outer − padding`, not the same radius (which bulges).
- [ ] **Consistent control heights**; buttons, inputs, selects share a height set (e.g. 40px).