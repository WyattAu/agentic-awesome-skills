description: Update StyleSeed engine in your project; analyzes what's outdated and updates safely
Use this skill when you need update StyleSeed engine in your project; analyzes what's outdated and updates safely.
- For just one new component or skin; copy that file manually
- For projects that have heavily diverged from upstream; manual diff review first
- For updating user code/components; this updates engine files only, not your custom UI
- `engine/VERSION` (or `version.json`) vs the local copy; the source of truth
- Skills present in `.claude/skills/` vs upstream (don't hardcode a count; list the diff)
- DESIGN-LANGUAGE.md: [location]; [old/current version indicator]
- theme.css; say "Your theme.css (skin) is untouched."
- components/; say "Your components are untouched. Run `/ss-lint` to check compliance."
- NEVER overwrite a project-specific CLAUDE.md; only MERGE the Golden Rules section