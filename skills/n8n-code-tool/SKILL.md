Expert guidance for writing code inside `@n8n/n8n-nodes-langchain.toolCode`; the tool an AI Agent can invoke, **not** the regular workflow Code node.
The Custom Code Tool looks like a Code node in the editor; same JavaScript editor, similar layout; but it is a **completely different node** from a different package with a **different runtime contract**.
| **Input** | `$input.all()`; item stream | `query`; string or object from LLM |
3. **Do NOT use `$fromAI()`** inside the Code Tool sandbox; it throws `"No execution data available"`.
4. **Do NOT use `[{json: {...}}]`** return format; that's for Code nodes. Throws `"Wrong output type returned"`.
6. **Write a precise description**; the LLM decides whether to invoke the tool based on it.
**Cons**: no schema validation; if the LLM forgets a field, the tool throws at runtime.
- `schemaType: "fromJson"` + `jsonSchemaExample` (n8n v≥1.3); paste an example JSON, n8n infers the schema
- `schemaType: "manual"` + `inputSchema`; write a full JSON Schema yourself
Errors don't just stop the workflow; they go back to the LLM, which usually corrects its call and retries. Use that:
// Option A: throw; n8n surfaces the message to the agent
// Option B: return an error string; agent reads it like any tool result
These fields are NOT documentation; they are the **tool contract the LLM sees**. Treat them as prompt engineering.
- The agent calls the tool by this name. `Code Tool` (the default) is useless; the agent won't know when to call it.
- If structured mode, the schema speaks for itself; just describe purpose.
**Fix**: `$fromAI()` is a helper for **other** tool-enabled nodes (HTTP Request Tool, SendGrid Tool, `toolWorkflow`, etc.); it's not exposed inside `toolCode`. Read the AI's input from `query` directly (or use `specifyInputSchema` for structured fields).
**n8n-code-javascript**: the Code **node** skill. Most JavaScript patterns (arrays, map/filter, DateTime) transfer; but I/O contract is different. Don't copy data-access code.
**n8n-validation-expert**: the three Code Tool errors listed above have clear signatures; if validation surfaces "Wrong output type returned", you know to switch from array-of-items to a string.
- [ ] **No `$input` / `$json` / `$helpers`**; those aren't in the sandbox
- [references/INPUT_SCHEMA.md](references/INPUT_SCHEMA.md); structured input (DynamicStructuredTool) in depth
- [references/ERROR_PATTERNS.md](references/ERROR_PATTERNS.md); full error catalog with causes and fixes
- [ToolCode source](https://github.com/n8n-io/n8n/blob/master/packages/%40n8n/nodes-langchain/nodes/tools/ToolCode/ToolCode.node.ts); the sandbox contract
- [LangChain tool docs](https://js.langchain.com/docs/modules/agents/tools/); DynamicTool / DynamicStructuredTool