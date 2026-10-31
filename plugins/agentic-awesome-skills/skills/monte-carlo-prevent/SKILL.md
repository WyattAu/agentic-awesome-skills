- Describes a planned change to a model (new column, join update, filter change, refactor) → **STOP; run Workflow 4 before writing any code**
  regardless of risk tier; do not skip the monitor offer
Present the results as context the engineer needs before proceeding; not as a response to a question.
### Macros and snapshots: gate edits, skip auto-context
macros are inlined into every model that calls them at compile time; a one-line
This applies whenever the user expresses intent to modify a model; including phrases like:
**Do not write or edit any SQL until the change impact assessment (Workflow 4) has been presented to the user.** The assessment must come first; not after the edit, not in parallel.
## Pre-edit gate: check before modifying any file
   **If risk is Low:** use judgment; proceed if straightforward
being changed in the current prompt; not just general table health.
the change"); proceed but note the skipped assessment.
| `getMonitors`                | Monitor configs; filter by table using mcons array                  |
### 4. Change impact assessment: REQUIRED before modifying a model
**What:** Generates 3-5 targeted SQL queries to verify the change behaved as intended. Uses Workflow 4 context; requires both impact assessment and file edit in session.
Do NOT say: "Proceeding with the edit."; that skips the engineer's decision.
(Use the model filename without .sql extension; NOT "acme.analytics.orders" or "prod.public.client_hub")
the assessment): Be strict; only emit markers for tables whose lineage **and**
marker when the gap is specifically about the columns or logic being changed -