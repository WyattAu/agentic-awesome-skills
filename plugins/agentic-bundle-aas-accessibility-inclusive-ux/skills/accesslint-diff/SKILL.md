description: Diff a live page's accessibility violations against a baseline; by default compares uncommitted changes (stash-based), or pass --branch [<name>] to diff against a branch. Reports only new violations introduced, violations fixed, and pre-existing count. Use `scan` for a full audit with no diffing.
Accessibility diff; http://localhost:3000/ vs main (94 rules, live DOM)
New; Critical
- color-contrast; 2.1:1 (needs 4.5:1), #bbb on #fff
- img-alt; <img src="old.jpg"> (no longer present)
Each new violation: **where** (selector verbatim + `file:line (symbol)` if `source` present; never fabricate), **evidence**, **fix** (mechanical change or `NEEDS HUMAN`).