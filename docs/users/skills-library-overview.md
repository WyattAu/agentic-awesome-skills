- [`CATALOG.md`](../../CATALOG.md); human-readable catalog grouped by category.
- `skills_index.json`; the legacy compatibility manifest (documented in [`discovery-manifest.md`](discovery-manifest.md)).
- `data/catalog.json`; structured catalog used by the catalog app and AAS Core.
1. **Search the full catalog**; with AAS Core running, use `search_skills`/`get_skill`; see the [usage guide](usage.md).
2. **Browse the generated catalog**; [`CATALOG.md`](../../CATALOG.md) lists every skill by category with description, risk, source, and tags.
3. **Use bundles as editorial shortlists**; see [bundles](bundles.md) for curated packs like the Web Wizard.
4. **Use the tool-specific guides**; [Claude Code](claude-code-skills.md), [Codex CLI](codex-cli-skills.md), [Cursor](cursor-skills.md), and [Gemini CLI](gemini-cli-skills.md) include starter picks.
- **Full library**; direct install of the complete catalog (`~/.agents/skills/` by default).
- **Plugins**; root and bundle plugins for Claude Code and Codex (see [plugins](plugins.md)).
- **AAS Core**; agent-owned, reproducible selection validated against the catalog.
- [Usage guide](usage.md); compose and use a skill stack.
- [AAS Core](aas-core.md); agent-owned catalog search and selection.
- [Discovery manifest](discovery-manifest.md); the machine-readable `skills_index.json` contract.
- [Bundles](bundles.md); curated skill packs.
- [Security skills](security-skills.md); guidance for security-relevant skills.