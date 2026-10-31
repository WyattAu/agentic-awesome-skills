- The user asked for the original Pi CLI (`pi`); use `pi-delegate`.
catalog is **this install's** authenticated providers; not a fixed list in this skill.
1. **List what this install can actually run.** Do **not** pass `omp --list-models`; that flag is
   - `omp models`; every available model, grouped by provider
   - `omp models --json`; the same catalog, machine-readable
   - `omp models find <substring>`; filter by provider, id, or name (example:
   - `omp models <provider>`; one provider's models
`inherit`) before dispatch; omp would otherwise warn and ignore a bad value.
passes `--yolo` (`tools.approvalMode: yolo`); otherwise a user's `always-ask` or `write` config
- Relay never commits; it only returns structured result JSON; you review and land the commit.
> Adapted from [amElnagdy/delegate-skills](https://github.com/amElnagdy/delegate-skills) (MIT); MIT; relay runtime bundled with local changes in UPSTREAM-NOTICE.md.