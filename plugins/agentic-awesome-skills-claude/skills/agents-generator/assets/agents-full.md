# AGENTS.md: Template (Full Mode)
> Compatible with the [agents.md](https://agents.md) standard (60k+ projects). Specific rules in `.agents/rules/`; {{RULE_CATEGORIES_LIST}}.
- Run `{{PRE_COMMIT_CMD}}` on changed files before committing; this is what CI runs.
- **stdout** is data; JSON, structured output, machine-readable.
- **stderr** is everything else; progress, warnings, hints.
and `build/` is rebuilt; never edit it directly. Stage generated files only
1. `cli/src/output.rs`; help output
2. `README.md`; options table, examples
3. `docs/`; MDX pages
| API error | Return as-is; never re-wrap |
- Budget: challenge each paragraph; does it justify its token cost?
- `npx vite build` hangs on NTFS; use `node node_modules/vite/bin/vite.js build`
- Server startup takes 30-60s; don't assume failure immediately
Do not bump versions in feature PRs; versioning is a release step.
- **Warn about large imports**: if a doc is over ~300 lines (~1500 tokens), flag it: "⚠️ Large file; consider importing only key sections."
If Claude Code is detected (`.claude/` directory or `CLAUDE.md` exists), also generate a thin `CLAUDE.md` at project root using `assets/claude.md`. AGENTS.md is the universal standard; CLAUDE.md only points to it and adds Claude-specific config if any exists.