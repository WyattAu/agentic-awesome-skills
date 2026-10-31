description: "Before accepting an agent's 'done / shipped / fixed' claim, verify it against ground truth (git ancestry + the commit's own diff) using the DOS kernel's `dos verify` and `dos commit-audit`; never the agent's own narration."
fact; and a claim the agent checks by re-reading its own work is *consistency,
commit's actual diff. DOS is deterministic; no API key, no LLM. The verdict is
- Use when folding many sub-agents' results; verify each claimed effect instead
- **Do not** use it to judge whether code is *correct*; that is what the test
even for a single `HEAD`), so read `verdict` from the first element; e.g.
diff; treat the "done" as unproven), or `ABSTAIN`. This judges the *kind* of
change, never correctness; run the tests for that.
`SHIPPED PLAN PHASE (via grep)` or `NOT_SHIPPED PLAN PHASE (via none)`; the same
**forgeability**; and forgeable evidence is exactly what this skill exists to
- `registry` or `grep-artifact`; **artifact-backed, still authored** (a registry row or artifact/diff match). Inspect the artifact and its provenance; presence alone does not close a behavioral claim.
- `grep-subject` (or bare `grep`); **forgeable**: a commit *subject* or body
  on an empty commit). Treat this as *shipped-per-the-subject*, not confirmed -
- `none`; no positive evidence; accept as "not shipped", not as a tool failure.
confidently the agent narrated it; send it back.
  text); corroborate before closing.
- ✅ Keep the test suite as the separate correctness gate; this skill checks shipping, not correctness.
- It checks whether a claimed change *shipped* / matches its diff; not whether the code is *correct*.
  it may shell a network check (e.g. `gh api`) for the verdict; pass `--no-ci`
  `dos-kernel` (the bare `dos` on PyPI is an unrelated package; do not install
  **Solution:** That is the honest "no evidence" verdict; it means the phase has