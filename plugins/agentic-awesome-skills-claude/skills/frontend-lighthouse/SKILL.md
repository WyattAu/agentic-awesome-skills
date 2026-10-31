> Portable skill; readable by Claude Code, OpenCode, Codex, Cursor, Windsurf, and others.
> This skill describes a **CI performance gate**; a Lighthouse CI config plus a workflow; not a
1. **One config, one source of truth.** All budgets and assertions live in a single `lighthouserc.cjs`. Named constants for each budget; no magic numbers buried in assertion objects.
with a comment explaining the threshold; never a bare number inside an assertion.
 * Lighthouse CI configuration; Core Web Vitals budgets for the marketing surface.
 * Core Web Vitals budgets on mobile; Google's "good" thresholds.
- `aggregationMethod: "median"` is non-negotiable; single-run gates flake constantly.
- Keep `onlyCategories` to exactly what you gate; fewer audits, faster, less noise.
aspirational. **Start strict and only loosen with a recorded reason**; a budget you keep raising
- `if: always()` on the upload step; you need the report most when the gate fails.
folder instead of `startServerCommand`; Lighthouse serves it internally.
- **Real regressions** → open the uploaded report artifact, read the failed audit's "Opportunities"/"Diagnostics", fix the cause (oversized image, render-blocking JS, layout shift from unsized media); don't just bump the budget.
- [ ] All budgets are named constants with units and comments; no magic numbers in assertions.
2. Keep the frontmatter `name` and high-signal `description`; discovery indexes match against it.