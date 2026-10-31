**Live mode** (explicit): when the user invokes this skill before writing docs, verify before you write; read the actual implementation, then document what it does. Run the self-check before delivery.
### Accuracy: must fix
1. **Every referenced symbol must exist.** Every function, method, class, hook, CLI command, flag, endpoint, config key, env var, and file path mentioned in the docs gets verified against the actual source, CLI help output, route table, or schema; by reading it, not recalling it. The verification procedure is in [references/verification.md](references/verification.md). An unverifiable reference does not ship.
2. **Runnable code samples must work; incomplete conceptual fragments must be explicitly labeled.** Imports resolve, APIs exist with the documented signatures (names, argument order, defaults, return shape), and the sample runs outside the author's machine; no hardcoded local paths, no real credentials, no implicit prior state. Sample rules: [references/code-samples.md](references/code-samples.md).
6. **A code change owes a docs change.** When editing code whose behavior is documented; rename, signature change, new default, removed flag; update every doc surface that mentions it in the same change. Grep the docs for the old symbol before finishing.
### Substance: should fix
8. **Don't paraphrase upstream docs.** Link to external documentation instead of restating it; paraphrased upstream docs drift the moment upstream changes. Document only your project's relationship to the external thing (which subset you use, what you configure differently).
9. **Examples cover the failure path too.** A tutorial that only shows the happy path documents half the API. Show what the error looks like and what the caller should do; using the error types the code actually raises (verify per Rule 1).
### Structure: worth noting
10. **Navigation tells the truth.** Headings describe their sections, the table of contents matches the actual headings, internal links and anchors resolve, and there are no TODO stubs or "coming soon" sections in published docs; unwritten sections are removed, not promised.
1. List every symbol, flag, endpoint, config key, and path your docs mention. Did you verify each one against the source in this session; not from memory?
Lead with Rule 1–4 findings (false claims), then drift, then substance. If a doc is clean, say so in one line; accuracy deserves credit.
- **Must fix:** Rules 1–4; false documentation is worse than no documentation; readers act on it
- **Should fix:** Rules 5–9; drift debt and noise that buries the signal
- **Worth noting:** Rule 10; navigation and polish
- [references/verification.md](references/verification.md); the mechanical procedure: extracting claims, verifying symbols, signatures, CLI flags, endpoints, config keys, links
- [references/code-samples.md](references/code-samples.md); what makes a sample shippable: runnability, realistic data, secrets hygiene, error paths
- [references/docstrings.md](references/docstrings.md); docstring/PHPDoc/JSDoc-specific rules: when one is justified, what it must contain, paraphrase detection
- [references/review-checklist.md](references/review-checklist.md); structured walk-through for review mode
- [references/sources.md](references/sources.md); research and style-guide URLs; read only when citing a source
- Review the code itself; clean-code-guard's jurisdiction. This skill reviews what the docs *claim about* the code.
- Generate documentation strategy or information architecture from scratch; it guards accuracy and substance, not scope decisions.
- Enforce a prose style guide; tone belongs to the project; truth belongs to this skill.