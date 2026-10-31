## Step 1: List available connection types
The script outputs JSON. Parse `result.connectors`; each entry has a `name` field. Present the names to the user and ask which connection type they want to build a config for.
## Step 2: Fetch the connector schema
- **`output_keys`**; the driver-level `connect_args` keys the mapper must produce (from the connector's `TypedDict`)
- **`default_field_map`**; the existing default mapping (credential field → Jinja2 template)
- **`default_steps`**; any default transform steps already configured
## Step 3: Optionally fetch available transform steps
Parse `result.transforms`; each entry has:
- `name`; the step type string used in `"type"`
- `step_input`; fields the step reads from the pipeline state
- `step_output`; derived fields the step writes, referenceable as `{{ derived.<key> }}` in the mapper
- `step_field_map`; typical mapper entry to wire the step's output into `connect_args`
**If the script fails:** Tell the user and offer to retry. You can continue without step data; just describe steps as unknown and ask the user to specify them manually.
## Step 4: Build the mapper
- **`raw`**; the flat credential dict as received. Use `{{ raw.field_name }}` to reference a credential field directly. Example: `{{ raw.client_id }}`
- **`derived`**; fields added by transform steps. Use `{{ derived.field_name }}` to reference a step's output. Example: `{{ derived.private_key_pem }}`
When the user doesn't know their credential field names, remind them these come from the Data Collector's credential dict; the keys are whatever the DC sends for that connection type.
## Step 5: Configure transform steps (optional)
| `when` | no | Jinja2 boolean expression; step only runs if this evaluates to true (e.g. `"raw.ssl_ca_pem is defined"`) |
| `field_map` | no | Mapper entries contributed only when this step runs; useful for conditional fields |
## Step 6: Output the final config
Remind the user that validation happens server-side via `validateConnectionCtpConfig`; they should test the config through that mutation (or the Validate button in the UI) after saving it.
- **`is not None` pattern.** An empty `field_map` (`{}`) is valid; do not treat it as missing. The monolith checks `ctp_config is not None`, not truthiness.