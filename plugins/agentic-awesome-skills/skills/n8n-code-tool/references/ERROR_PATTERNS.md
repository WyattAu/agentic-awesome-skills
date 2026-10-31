**Cause**: Calling `$fromAI()` inside the Code Tool sandbox. `$fromAI()` is a helper intended for *other* tool-enabled nodes (HTTP Request Tool, SendGrid Tool, `toolWorkflow`) where AI-supplied values flow through workflow execution data. The Code Tool sandbox has no execution data; it receives input directly via `query`. The helper throws, n8n tries to annotate the error's `name` property, and that assignment fails because the error object is frozen.
// ✅ Unstructured; parse a JSON string
// ✅ Structured; with specifyInputSchema: true
**Numbers are fine**; n8n auto-converts them to strings:
**Booleans are NOT auto-converted**; stringify explicitly:
- Need other-node data or credentials? Move to **Call Sub-workflow Tool (`toolWorkflow`)**; its sub-workflow has a full Code node sandbox.
## Error 7: Python-specific: `"name 'query' is not defined"`
- [ ] **No `$input`, `$json`, `$helpers`**; not in this sandbox
- **Use the Execution view**, not just the test output. The agent's tool invocation and raw input/output are visible there; you can see exactly what `query` the LLM sent.