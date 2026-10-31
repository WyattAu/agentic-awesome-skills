This file is about the **error output on a single node**; the second `main` output that fires when that node throws; and the two-step setup that trips up nearly everyone. For the workflow-level catch-all (Error Trigger workflows) and the webhook/Respond shape, see the rest of `n8n-error-handling`.
The whole point: a node failing should route somewhere *you* control, instead of halting the run. The cost of forgetting half the setup is one of the worst silent-failure modes in n8n; a run that shows green while quietly dropping its work.
### Step 1: create the error output
| `"continueRegularOutput"` | The error item flows out the **normal** output (`main[0]`) alongside successes. Rare and usually a mistake; downstream gets error-shaped data and keeps going. |
### Step 2: wire the error output
`sourceIndex: 1` is the error output. (IF nodes accept the friendly aliases `branch: "true"`/`branch: "false"` for index 0/1; a generic fallible node has no such alias; use the explicit `sourceIndex: 1`.)
## Failure modes: why "one of two" is so dangerous
On failure the node emits to `main[1]`, which has **no targets**. The error data is silently discarded, downstream never fires, and; this is the trap; the execution is recorded as **succeeded**, because from n8n's perspective the error was "handled" by a branch that happens to go nowhere. No failed execution logged, nothing in the dashboard. The integration "just stops working" and there's no trail.
The connection sits in the JSON, but the slot it feeds from never fires. The handler is unreachable. On failure the workflow simply **halts** (default behavior). Less dangerous than the first mode; at least it's loud; but the handler you built does nothing.
A half-wired error output **validates clean**. `validate_workflow` and `n8n_validate_workflow` don't flag "`onError` is set but `main[1]` is empty" or vice versa; both are structurally legal. This is a runtime behavior, not a schema violation. The only reliable check is to read the workflow back (see Verification below).
Wiring the error output to two targets composes without conflict; both receive the error data:
- Network calls; HTTP Request, third-party API nodes, databases.
- Auth failures; expired credential, rotated token.
- Schema mismatches; missing DB column, JSON parse failure.
- Rate limits; 429 from upstream (configure `retryOnFail` first so these self-heal).
- File/binary operations; missing path, permission denied (see **n8n-binary-and-data**).
- IF / Switch with simple expressions; if those throw it's a bug to fix, not a path to catch.
1. **Node config**; `onError` is `"continueErrorOutput"` (or whatever you intended).
2. **Connections**; `connections["<node>"].main[1]` contains the expected handler(s).
`n8n_autofix_workflow` can repair some structural issues, but it won't infer that you *meant* to wire an error path; the intent to handle a given node's failure is yours to express. Treat the read-back as mandatory.
For those, you need a workflow-level **error workflow** (Error Trigger node). And note the inverse: a per-node error output that's wired to a no-op which drops the data counts as "handled"; so it will *suppress* the error workflow. Only catch per-node when you're genuinely acting on the error. See **ERROR_WORKFLOWS.md**.