description: Design; spatial composition
# Design: spatial composition
Use this skill when you need design; spatial composition.
UI is generated as a token stream, never as pixels; so the model cannot perceive collisions, overlap, imbalance, or broken spacing. It will write a headline that runs into the hero image and have no idea.
**Critique with fresh eyes; not your own.** Grading your own output rationalizes it; the builder looks at its overlapping headline and calls it fine (this is exactly how a real collision shipped in testing). Use a separate judge; a subagent that did *not* write the page; and tell it to hunt for what's *wrong*: collisions, edge tangents, ragged alignment, lopsided weight, no clear focal point, breaks at some width. Fix, re-render, re-judge.
Whatever it produces first is the mean of its training data; and there is more than one mean:
Landing on the second isn't taste; it's a more flattering average, which is why it slips past. **Treat your first instinct as the mean and deviate deliberately; toward *this product's specific world*** (use design-thinking's domain / color-world / signature as the direction), **not toward another trend.** If the result could be any startup, you shipped the mean.
Any fixed rule; a 12-col grid, an 8-point scale, "mono = data"; *becomes* next cycle's mean, and a blind model executes it into collisions anyway. Prescribe the **process, not the look**: see it with fresh eyes, and push off the average toward the domain. Taste supplies the direction (design-thinking / design-philosophy); this skill only insists you **look** and **don't ship the mean**.
## 4. NEVER ship horizontal overflow: THE mandatory gate, no exceptions
> it isn't done; say "haven't checked overflow yet" instead of claiming done.**
viewport is wide enough to hide it; the overflow only appears once the window is
new item to a `flex`/`inline` row, you have invalidated the last overflow check; the
page 309px across 720–1200px; both invisible at dev width, both caught only by
**The check; run before calling ANY page done:** `document.documentElement.scrollWidth - document.documentElement.clientWidth` must equal `0`, tested at your dev width AND resized narrow (≤1024px, and a phone width ~390px). If > 0, find the offender:
**Safety net:** `overflow-x: clip` on `body` (prefer `clip` over `hidden`; it clips without creating a scroll container, so it won't break `position:sticky`/anchored layouts). But a net is not a fix; **find and kill the root cause:**
- **`position:absolute` + `white-space:nowrap` anchored at an edge** (`left:100%`, `right:0`): a *centered* nowrap label on the right edge juts past the viewport. (Real ship, 2026-06: a progress bar's "300 · learned model" milestone label at `left:100%` with `translateX(-50%)` overflowed 23px → phantom horizontal scroll at sub-1180px widths.) **Anchor edge labels inward**; right end `right:0; transform:none`, left end `left:0; transform:none`.
- **`100vw`**; includes the scrollbar width (~15px), so on any vertically-scrolling page it guarantees ~15px of horizontal overflow. Use `100%`.
- **flex / grid children without `min-width:0`**; they refuse to shrink below their content and blow out the track (a long title in a flex card, a `<pre>` in a grid cell). Add `min-width:0`.
The generalization: **anything pinned to an edge or sized in viewport units is a horizontal-overflow suspect; test narrow, measure `scrollWidth`, clip the body as backstop, and anchor edge-pinned content inward.**
## 5. Lay out in TASK order: minimize transition cost
- **Orient at top:** controls/options up top are good; they tell the user what
  bottom; where the user's eyes and cursor are when they finish. The original
  failure: a delete-review page with confirm buttons only in the top toolbar -
  control?"; a layout can be aligned, balanced, and still force a round trip.
## 6. Balance is measurable: don't eyeball it (or trust a VLM's eye)
balance**; ask a VLM "is this centered / balanced?" and it confabulates a verdict.
offline pixel-oracle that re-measures the rendered screenshot; see the
arithmetic, not taste; so compute it.
**Optical center, not geometric.** Target `x = 0.50`, `y ≈ 0.46`; slightly high,
per project; these were hand-guesses until corrected against the pixel oracle):
auto-balancer's escalation order in `asym.html`; grow the opposing heading first
**Two models; and why you need the independent one:**
  *center*. Instant, fine for dragging sliders. BUT it has a systematic bug -
  nothing about the layout's intent; it just counts ink. **When the box model and
balance-critical layout "balanced", do NOT assert it from the code or a VLM opinion -
## 7. The layout audit: metrics that MEDIATE the eye, never replace it
`browser_evaluate` on a rendered page. It measures six things deterministically; all
**What makes it mediate rather than replace:** it doesn't just return JSON; it **draws
**These are HEURISTICS, not laws; and they split into two kinds you must not conflate:**
  (Collision is a near-gate: usually a real bug, but can be intentional; so eye-confirm,
  closely the layout matches a *symmetric, regular, gridded* aesthetic; which is exactly
**The discipline (the whole point; bias hard toward this):**
  you didn't mean, a phantom scrollbar, a 1.9:1 caption; catch those. But the *same*
  banner, asymmetric hero; the metric flags them; common sense overrules. Proven live in
  clears the deterministic floor; it does **not** mean the layout is good. A bland centered
## 8. Optical craft: perception beats geometry
it reads perception; so a few cases need a manual nudge the metric can't make. These are
- **Optical alignment; nudge ±1–2px when it *looks* off though it measures centered.** A
  vertically centered by box metrics frequently sits a hair low; lift it. Geometry is the
- **Balance icon/text lockups.** When an icon sits beside text, match their *visual weight* -