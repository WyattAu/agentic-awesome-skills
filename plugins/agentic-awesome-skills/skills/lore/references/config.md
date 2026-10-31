  "mirror_targets": ["CLAUDE.md"], // optional; auto-detected if absent
- **Higher than expected** → warn and continue: `list_entries.py` prints a `[WARN]` to stderr and proceeds with best-effort reads (readers are forward-compatible; see `references/compatibility.md`). The user's skill is older than their `.lore/`; recommend pulling the latest lore from upstream.
Controls whether `compress` regenerates platform mirrors automatically after it writes `SUMMARY.md`. It does **not** gate the explicit `lore mirror` command; `lore mirror` always regenerates (with content-based dedup) once `mirror_targets` is resolved.
- `true`; `compress` regenerates mirrors automatically
- `false`; `compress` asks per target before writing
- `false`; `sync` only writes `.lore/*.md`. Mirrors are updated by `compress` or explicit `lore mirror`. This is the recommended setting to avoid cluttering `git log` of mirror files.
- `true`; `sync` regenerates mirrors (with content-based dedup) after the canonical change is accepted. Restore this setting if the old "update everything on every sync" behavior is preferred.
- `"high"`; auto-apply everything, including `NEW` and `STALE`. Only `ALERT` blocks interrupt.
- `"medium"`; auto-apply low-risk changes (de-duplicate hits, tags-only REFINEDs). Body-changing REFINEDs, `NEW`, `STALE`, and `ALERT` require confirmation.
- `"low"`; every change requires confirmation, including de-duplicate hits and tags-only REFINEDs.
The git commit SHA from which the next `sync` will compute its delta. Written automatically by `sync` after every successful `.lore/*` update. Optional and additive; old configs without it keep working.
- **Absent or `null`**; `sync` falls back to working-tree diff alone (`git diff`, working tree vs. `HEAD`). Already-committed changes between two syncs are invisible.
- **Set to a reachable SHA**; `sync` uses `git diff <last_sync_sha>..HEAD` for committed changes plus working-tree diff for uncommitted changes. This is the recommended state and lets batched commits be captured by a single later sync.
- **Set to an unreachable SHA** (e.g., after `git rebase` or a force-push that orphaned the SHA); `sync` prints a `[WARN]` to stderr and falls back to the working-tree diff alone. The next successful sync resets the baseline.
- **Empty repo** (no commits yet); the field is `null`; only the working-tree diff applies.