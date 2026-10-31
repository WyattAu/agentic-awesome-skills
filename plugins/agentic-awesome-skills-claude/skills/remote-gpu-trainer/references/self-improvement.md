# Getting better over time: capture new gotchas + personalize (without corrupting the skill)
something; a new platform quirk, a training bug not in the catalog, or the user's own setup. This file
1. The bar; what qualifies as a keepable gotcha
2. Route the learning; user memory vs the catalog vs an upstream PR
4. First-run personalization; capture the user's setup into memory
5. Freshness; platform facts rot; stamp and re-verify
## 1. The bar: do NOT enshrine a one-off
- **Root-caused**; the mechanism is understood, not just "it worked after I retried."
- **Reproduced or clearly mechanistic**; not a single flaky incident (a transient network blip is not a gotcha).
- **Generalizable**; another user on this platform / training setup would hit it too.
| **User/personal/host-specific**; this account's quirk, the user's preference, the usual GPU plan, "on MY box X is true" | **the host's `memory/` system** (host-specific, personal, may be ephemeral) | a `reference` / `project` / `feedback` fact, one per file, deduped |
| **A project-level fact or recurring error the user keeps hitting on THEIR project**; a config quirk, "always run X first", a path that must be Y, an env that must be activated | a **project instructions file in the user's repo**; `CLAUDE.md` / `AGENTS.md` / `.cursorrules` (persists cross-session AND cross-tool AND for collaborators, unlike host memory) | a short imperative rule; **propose, don't auto-write** (§3) |
**upstream PR**; offer to open one so every user benefits, not just this install.
**NEVER silently rewrite a skill file from a single run**; a wrong "fix" or a broken structure is worse
- Apply only after the user okays it; then re-run the skill's own checks; cross-refs resolve, **no secret
- the tracker entity (wandb / trackio) and **where the key lives** (its env-var name or file path; never the value);
## 5. Freshness: this skill has a shelf life
<month>` at authoring time. Before betting **money or data** on a teardown/billing fact; the
irreversible ones (`terminate` / `destroy` / release); **re-verify it against the platform's current
honest; schedulable via the host's `/schedule`. Run `scripts/check_staleness.py` to list every `verified`
stamp older than N months (a mechanical reminder of WHAT to re-check; it does not verify the fact itself).