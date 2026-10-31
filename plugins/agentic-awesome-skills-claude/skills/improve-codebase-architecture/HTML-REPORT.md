The architectural review is rendered as a single HTML file (the CDN version requires network access) in the OS temp directory. Tailwind and Mermaid both come from CDNs. Mermaid handles graph-shaped diagrams reliably; hand-built divs and inline SVG handle the more editorial visuals (mass diagrams, cross-sections). Mix the two; don't lean on Mermaid for everything, it'll start to look generic.
    <title>Architecture review; {{repo name}}</title>
Repo name, date, and a compact legend: solid box = module, dashed line = seam, red arrow = leakage, thick dark box = deep module. No introduction paragraph; straight into the candidates.
- **Title**; short, names the deepening (e.g. "Collapse the Order intake pipeline").
- **Badge row**; recommendation strength (`Strong` = emerald, `Worth exploring` = amber, `Speculative` = slate), plus a tag for the dependency category (`in-process`, `local-substitutable`, `ports & adapters`, `mock`).
- **Files**; monospaced list, `font-mono text-sm`.
- **Before / After diagram**; the centrepiece. Two columns, side by side. See patterns below.
- **Problem**; one sentence. What hurts.
- **Solution**; one sentence. What changes.
- **Wins**; bullets, ≤6 words each. e.g. "Tests hit one interface", "Pricing logic stops leaking", "Delete 4 shallow wrappers".
- **ADR callout** (if applicable); one line in an amber-tinted box.
Pick the pattern that fits the candidate. Mix them. Don't make every diagram look the same; variety is part of the point.
Modules as `<div>`s with borders and labels. Arrows as inline SVG `<line>` or `<path>` elements positioned absolutely over a relative container. Reach for this when you want the "after" diagram to feel like one thick-bordered deep module with greyed-out internals; Mermaid won't render that with the right weight.
Two rectangles per module; one for interface surface area, one for implementation. Before: interface rectangle is nearly as tall as the implementation rectangle (shallow). After: interface rectangle is short, implementation rectangle is tall (deep).
- Use `text-xs uppercase tracking-wider` for module labels inside diagrams; they should read as schematic, not as UI.
- The only scripts are the Tailwind CDN and the Mermaid ESM import. The report is otherwise static; no app code, no interactivity beyond Mermaid's own rendering.
Plain English, concise; but the architectural nouns and verbs come straight from the `/codebase-design` skill. Concision is not an excuse to drift.
- "Order intake module is shallow; interface nearly matches the implementation."
**Wins bullets** name the gain in glossary terms: *"locality: bugs concentrate in one module"*, *"leverage: one interface, N call sites"*, *"interface shrinks; implementation absorbs the wrappers"*. Don't write *"easier to maintain"* or *"cleaner code"*; those terms aren't in the glossary and don't earn their place.