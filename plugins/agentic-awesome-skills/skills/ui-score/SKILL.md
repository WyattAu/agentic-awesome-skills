description: Score a UI file's design quality 0-100 against StyleSeed's design language; per-category breakdown, the worst offenders, and a prioritized fix list. A quantified version of /ss-review.
Use this skill when you need score a UI file's design quality 0-100 against StyleSeed's design language; per-category breakdown, the worst offenders, and a prioritized fix list. A quantified version of /ss-review.
overall* and *what to fix first*; a single number plus a category breakdown, so
- For non-UI files (logic, config); scoring is meaningless
reading the code. Be specific and evidence-based; cite the line.
**Color discipline (18)**; deduct for: any `#000`/`text-black` (−4 each, cap −8);
**Hierarchy & typography (18)**; deduct for: number/unit not ~2:1 (−4); font
**Layout & rhythm (14)**; deduct for: content on bare background, not in cards
**Cards & elevation (12)**; deduct for: 1px borders doing separation work that
**States & a11y (18)**; deduct for: missing empty/loading/error state on a data
**Motion & interaction (8)**; deduct for: random/ad-hoc fades instead of a named
**Coherence (12)**; the "one choice per axis" laws (VISUAL-CRAFT §C0). Deduct for
(−2). This is the category that most predicts "looks AI-generated"; weight evidence
The pass bar is a **floor, not a ceiling**; get to ≥ 80 and stop; don't chase 100. The point
80 with a rainbow status list, emoji icons, two accents, or missing states; those are the
- **Read the file**; score from real evidence (line numbers), never guess.
- As a *gate*, ≥ 80 is a floor before showing the user; but don't over-polish: chasing 95→100