description: Find and fix WCAG 2.2 accessibility issues. Two modes; report (sweep a codebase or page, produce a prioritized written report, no edits) and fix (audit→edit→verify loop on a target). Prefers direct-CDP live-DOM auditing; falls back to a browser-MCP composition or HTML-string audits.
- **Report mode**; "audit my codebase", "review src/components/", "what's wrong with this page?", "give me an a11y report". You audit + write a report. **You do not edit files.**
- **Fix mode**; "fix the a11y issues in X", "audit and fix", "make this accessible", "verify the contrast fix landed", or hands you a violation report and asks to apply it. You audit → edit → verify.
1. **`audit_live`**; try first for any URL. Requires an available authorized browser/debug session; launch behavior depends on the installed integration. Single call; IIFE bytes don't enter your context.
2. **`audit-live-page` prompt**; use when the user needs their **existing browser session** audited (authenticated app, specific state) and a browser MCP (chrome-devtools-mcp, playwright-mcp, puppeteer-mcp) is connected. Invoke via `Skill` with `mode: "fix"` or `mode: "plan"`.
3. **`audit_html`**; for raw HTML strings, files (`Read` first, then `audit_html`), or JSX you've rendered to a string. Pair with `audit_diff({ html })` for fix-mode verification.
- **Directory path**; analyze all relevant files within.
- **Multiple files**; analyze the listed files plus imports they reach.
- **A URL**; audit it. If it's a dev-server URL, that's flow 1 or 2.
- **No arguments**; ask the user to narrow scope. Whole-codebase sweeps are rarely the right thing.
2. **Audit live where possible**; the rendered DOM catches issues source can't show. Use the flow picker above.
3. **Look for patterns.** If one component fails a rule, similar components likely do too. Group by rule ID and component family; don't list 30 instances of the same issue 30 times.
The engine catches what's mechanically detectable. Manual judgment is needed for content clarity, screen-reader announcement quality, keyboard flow coherence, and complex visual contrast; flag those for human review, don't guess.
# Accessibility audit: <scope>
- **WCAG**: <ID>; <name>
What the codebase does well; short, factual, reinforces practices to keep.
`Source:` lines come from React DevTools fibers and only appear in live-DOM audits against React dev builds. Static audits won't have them; fall back to selectors.
- A violation has no `Fix:` directive; leave a `TODO`, don't guess.