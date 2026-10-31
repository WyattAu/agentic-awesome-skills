# Execution Explain: Step-by-Step Guide
Apply the **Premises Construction Checklist** at `../_shared/semiformal-checklist.md`; Name Resolution, Type Contracts, State Preconditions, Control Flow Assumptions.
Format: "Worth noting: `format()` on line 42 is NOT Python's builtin; it resolves to the module-level `format()` at line 8 of this file, which expects a datetime object."
- **Findings section:** Always omit; logic-explain produces no L-code findings and no Remedy. If the trace reveals a bug, note it in Step 4, recommend re-running with logic-review or logic-locate, and output a handoff block:
  Suspected divergence: [one-sentence description; no L-code, no Remedy]