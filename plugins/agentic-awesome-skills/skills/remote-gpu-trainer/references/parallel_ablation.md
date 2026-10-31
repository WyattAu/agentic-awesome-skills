# Parallel Ablation Fan-out: FS-shared deployment, isolated write paths, reconciliation
Parallelism comes from running **multiple queues on multiple instances simultaneously**; never from
Split the N cells across queue files (one per instance) by cost, not count; route the long cells
Volume, on a bare box a synced NFS/`rsync` target). Every instance reads the **same version**; no
**Recall principle #6; never mutate inputs under a live run.** A running queue holds
**REQUIRED:** `superpowers:dispatching-parallel-agents`; fan out only over work whose units share no
nothing else.** The per-job output dir is the platform analogue of a **git worktree**; an isolated
`checkpoint_dir`, or one tracker run id = the exact shared-state violation the predicate forbids -
What is read-shared (the immutable wrappers, the dataset, the base image) is fine; the predicate
against `profiles/<platform>.md`; the profile supplies paths/verbs, the job supplies the work; see
- `cfg_path`; yaml file relative to repo root; its basename is the cell `<name>` (the isolation key).
- `task`; reconstruction / segmentation / detection (or other supported task); sets tracker group/tags.
- `epochs`; optional integer; omitted → wrapper default (e.g. `20`). The optional 3rd field lets one
`bash run_queue.sh queue.txt <k>`; it skips the first k−1 non-comment cells and starts cell k. This is the queue-level
appending lines is safe, **reordering or deleting earlier lines shifts every index**; append only.
queues report done, the watcher's "done" is a **claim** (principle #3), not ground truth; a cell can
Reconcile and re-verify **every cell before any teardown**; this is a hard gate, not a spot check:
   violation that slipped through; see Gotchas).
3. **Re-verify by load.** Run `scripts/verify_local.py` over the durable copies; *load* each
   is real; **REQUIRED:** `verifying-dl-experiments`).
   fresh queue. Do not patch one cell's config to make it pass; that destroys comparability.
output dirs for N cells. → Root cause: independence-predicate violation; two queue lines mapped to
rename one cfg and rerun both; interleaved output cannot be un-mixed after the fact.
Root cause: principle #6; live bash holds the script by byte-offset; overwriting the shared copy
died (released, preempted, host fault) and its queue's "done" was never emitted; absence of failure
> many-small-files eval output across a shared FS) are **not** restated here; see