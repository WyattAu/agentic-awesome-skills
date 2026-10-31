Every framework-specific code decision must be backed by official documentation. Don't implement from memory; verify, cite, and let the user see your sources. Training data goes stale, APIs get deprecated, best practices evolve. This skill ensures the user gets code they can trust because every pattern traces back to an authoritative source they can check.
If versions are missing or ambiguous, **ask the user**. Don't guess; the version determines which patterns are correct.
Fetch the specific documentation page for the feature you're implementing. Not the homepage, not the full docs; the relevant page.
**Not authoritative; never cite as primary sources:**
- Your own training data (that is the whole point; verify it)
A) Use the modern pattern (useActionState); consistent with current docs
B) Match existing code (useState); consistent with codebase
- Prefer deep links with anchors where possible (e.g. `/useActionState#usage` over `/useActionState`); anchors survive doc restructuring better than top-level pages
| "The docs won't have what I need" | If the docs don't cover it, that's valuable information; the pattern may not be officially recommended. |