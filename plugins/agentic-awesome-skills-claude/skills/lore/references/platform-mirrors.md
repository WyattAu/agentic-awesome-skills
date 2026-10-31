| Cline | `.clinerules` (root) |; |
| OpenAI Codex | `AGENTS.md` (root) |; |
| OpenCode | `AGENTS.md` (root) |; |
| Windsurf | `.windsurfrules` (root) |; |
| GitHub Copilot | `.github/copilot-instructions.md` |; |
| Continue.dev | `.continue/rules/lore.md` |; |
| LangGraph / DeepAgents | (no file; inject at runtime) |; |
For LangGraph and DeepAgents, the skill does not produce a mirror file. Read `.lore/*.md` directly or ingest into the system prompt at runtime; that ingestion is the user's responsibility.
This is the core resolution used by all three commands. `init` extends it with classification and per-file takeover steps; see "Init-time behavior (full procedure)" below.
These match the platform table above (default + "Also accepted" filenames). The `.cursor/rules/*.mdc` entry is a glob; it's a hit if `.cursor/rules/` exists and contains at least one `.mdc` file.
Aider, Codex, and OpenCode all map to `AGENTS.md`. Selecting any combination produces one entry. Selecting nothing is valid; writes `mirror_targets: []` (no mirrors generated).
- `lore init`; always interactive.
- `lore mirror` when `mirror_targets` is absent; also interactive (skill is invoked through chat).
- `lore compress` (when `auto_mirror: true`); also goes through this resolution if `mirror_targets` is absent.
- Monorepo with pnpm workspaces + Turborepo; [_global/ARCHITECTURE.md#ARCH-2026-01-15-d7a3]
**`sync` does not regenerate platform mirrors.** This is intentional; see the "Mirror update triggers" section in `SKILL.md`. The skill only writes `.lore/*.md` during `sync`. To update mirrors after `sync`, the user runs `lore mirror` (or `compress`, which calls mirror generation as a side effect).
The content-based dedup step (4) is the key reason `mirror` can be run frequently without polluting `git log`; most invocations will be no-ops once the mirror is in sync.
   - **Class (a)**; already a lore mirror: contains `## Lore` section.
   - **Class (b)**; user-written: contains `## My notes` but no `## Lore`.
   - **Class (c)**; unmarked: neither header present.
4. **Multi-select question.** "Which agents do you use in this project?" Default pre-selection: every agent corresponding to a class (a) file. Empty selection is allowed; but class (a) files still get included via Step 5. Unlike `mirror` / `compress` (which follow the resolution algorithm's silent return-on-detect), `init` always asks; this is how the user adds agents whose files don't exist yet.
The mirror's Lore section is an **index** into `.lore/`; not a copy of its content. This keeps per-session token cost flat (size proportional to the number and length of scope descriptions, rather than copying all entry text) and aligns with how platform instruction files (`CLAUDE.md`, `.cursorrules`, etc.) are designed to be used: as small pointers that tell the agent where to find detail on demand.
The `<!-- LORE:START -->` / `<!-- LORE:END -->` markers, `## Lore (auto-managed)` opener, `---` separator, and `## My notes (free edit)` closer are **always present** in new mirrors; only the `**Structure**:` body varies with adaptive rendering. Agent preserves the My notes section verbatim across regenerations.
- `<scope_name>`; directory name under `.lore/scopes/`. Each scope's full path is `.lore/scopes/<scope_name>/`.
- `<description>`; extracted from `.lore/scopes/<scope_name>/ARCHITECTURE.md` via the HTML comment `<!-- description: ... -->`. See "Scope description extraction" below. If absent, the description is omitted (scope row still appears, just without parenthetical).