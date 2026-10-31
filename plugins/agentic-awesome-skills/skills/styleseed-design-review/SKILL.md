description: "Reviews UI/frontend code and tells you exactly why it \"looks AI-generated\"; then how to fix it."
don't agree with each other**; mixed corner radii, three accent colors, pure-black text,
### 1. Coherence: 20  (the #1 "AI-generated" tell)
- mixed corner radii; e.g. a sharp card with pill buttons (−6)
- **emoji used as UI icons** (🚗🧺⭐ as list/nav/status/category markers); injects many uncontrolled hues; use one line-icon set in currentColor (−6)
### 2. Color discipline: 16
- pure black (`#000` / `text-black`) text; the refined black is ~`#2A2A2A` (−4 each, cap −8)
- **status color on most/every row** (no severity hierarchy; color should mark the minority that needs attention) (−4)
- **decorative hues**; gold stars, rainbow category dots, a different color per card; instead of accent/grey (−3)
### 3. Hierarchy & typography: 16
### 4. Layout & spacing: 12
### 5. States: 12
### 6. UX writing: 12
### 7. Motion & polish: 12
  re-review until the score clears ~80 *before showing the user*; no first-draft, incoherent
Based on **StyleSeed**; an open-source (MIT) design engine that gives Claude Code, Cursor,