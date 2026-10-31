- The project already has a `CONSTRAINTS.md` and the user isn't changing it; read it and follow it instead
- Pre-product-market-fit code with a two-week expected lifetime; the floor below is still worth it, the rest isn't
GUESS: (a) and (b); you have a test runner already and you're handling user input.
Picking a dimension means installing something. Don't leave the user with a number and no mechanism, and don't invent your own checker when a de facto one exists; these tools are listed because their rule formats and thresholds are what everything else in the ecosystem targets, so the team's existing config keeps working.
2. **Lighthouse and axe need a URL.** They only work against a running app, so they belong in the runtime stage against a preview deploy or a local server you start first. If the project has no URL to hit; a CLI, a library, a desktop app; say so and drop the dimension rather than inventing a check that can't run.
The commands now live in two places; the `Checked by` column in `CONSTRAINTS.md` and these scripts. `CONSTRAINTS.md` is the canonical source: it carries the reason alongside each command and it shows up in review. The scripts are convenience wrappers that must mirror it, not a second source of truth; if they drift, the file wins.
- **External**; axe-core encodes WCAG, `osv-scanner` reads a vulnerability database, Lighthouse measures a real browser. Their coverage and configuration still require review.
- **Project**; your lint rules, your layer boundaries. A human owns the file.
- **Suite**; your own tests. Most useful, and the only genuinely circular one.
- `interview-me`; the one-question-at-a-time discipline this skill's intake borrows
- `code-review-and-quality`; how to review; this skill decides what the review enforces
- `ci-cd-and-automation`; building the pipeline these constraints run in
- `test-driven-development`; the suite that coverage and mutation constraints measure
- `security-and-hardening`; what the security dimension should contain
- `performance-optimization`; where the performance numbers come from