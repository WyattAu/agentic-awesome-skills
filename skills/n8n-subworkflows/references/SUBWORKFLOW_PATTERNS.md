For a body that just processes items the ordinary way; map, filter, transform; the two are equivalent, because n8n nodes iterate per-item regardless of how many items arrived.
- **Per-run aggregation.** A node like "sum these line items" or "build one report from these rows" produces a single output from whatever items it sees. Under `mode: all` it sees all N inputs and produces *one* aggregate across everyone. Under `mode: each` it runs N times and produces one aggregate *per input*; which is almost always what a per-customer / per-order body means.
- The body stays single-item and simple; no batch-cursor logic, no cross-iteration state to manage.
- **Binary vs non-binary input** (the canonical one; typed fields are JSON-only).
1. Pick passthrough (most permissive; it supports binary).
- The workflow can't be exposed as a clean agent tool; passthrough has no `$fromAI` schema.
For N divergent input contracts, build **N+1** sub-workflows: one *outer* per contract, plus one *shared downstream* for the common work. Each outer does its input-specific prep; validation, fetching, normalization, hashing, extraction; and calls the shared core with a normalized shape. The shared core has a single typed input contract and knows nothing about which outer called it.
  Trigger: Passthrough   (required; binary flows through)
The "pull" path (look up by ID) and the "push" path (data already in hand, here as binary) each get their own typed-or-passthrough trigger, and converge on one typed core. Add a third input shape later and you add a third outer; not a third branch.
The catch: the caller doesn't know when; or whether; any of them finished. So this only works with a **separate completion-tracking mechanism**, typically a Data Table the sub-workflow writes to as it progresses (manage it with `n8n_manage_datatable`; see **n8n-mcp-tools-expert**).
3. **Each sub-workflow.** Does its work, then updates *its* row; `status: completed` / `error`, plus output.
If a sub-workflow crashes without updating its row, the poll sees `inProgress` past the runtime cap and times it out; so a dead job can't hang the loop forever.
- **You can afford eventual consistency**; the poll loop adds latency by design.