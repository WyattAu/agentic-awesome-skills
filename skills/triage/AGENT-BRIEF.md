The brief states **what the agent should do**, which stretches to both surfaces: for an issue, that's building the change from nothing; for a PR, it's what's left to do *to the existing diff*; finish it, close gaps, address review points. Same principles either way; the PR example below shows the difference.
- **Don't** reference file paths; they go stale
- `TypeName`; what needs to change and why
- `functionName()` return type; what it currently returns vs what it should return
- Config shape; any new configuration options needed
- The `SkillMetadata` type's `description` field; no type change needed,
- Markdown file format in `.out-of-scope/`; each file should have a
With `--json`, all output; including errors; is well-formed JSON on stdout,