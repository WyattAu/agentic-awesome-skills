- What is the firing cadence; multiple times per day? Daily? Sporadic?
This applies to any monitor that uses ML thresholds; both metric monitors and custom SQL monitors.
(they have their own per-metric sensitivity; see the table monitor reference), and for monitors
Output a structured analysis. **This is the primary output; include it in full.**
**Type:** {monitor type; metric, custom SQL, validation, or table}
{1-3 sentence summary of what the alerts represent; operational events, bursty data, model
{Any configurations that look correct and should be left alone; avoid over-tuning.}
1. **Always preview first**; show the user what will change before applying.
3. **Validate the preview YAML against the schema**; before presenting the preview YAML to the user, fetch the published MaC JSON Schema from `https://clidocs.getmontecarlo.com/mac/schema.json` (WebFetch) and check the preview YAML against it. If any field in the YAML does not appear in the schema for the given monitor type, flag it and correct it. Note: the schema validates field names, types, and enum values only; schema constraints vary by version; do not assume validation proves backend compatibility or safe detection. Inspect the installed tool schema too, use a safe YAML loader and retain schema version/hash. If the schema is unavailable, do not claim validation passed or deploy an unverified field.
4. **MaC-managed monitors**; if `get_monitors` returns a `mac_name` or the user mentions the monitor is managed via a MaC YAML file, note this before applying: changes made via the API will be overwritten the next time `montecarlo monitors apply` runs. Offer to hand off to `/manage-mac` (edit workflow) instead so the YAML file stays the source of truth.