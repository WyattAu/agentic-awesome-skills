The agent picks tools by reading their **name** and **description**; nothing else. Both are part of the prompt. Treat tool design like API design: what it does, when to use it, what each parameter means, and how it fails.
When a native node is missing an operation or needs a non-standard param shape, point an **HTTP Request Tool** at the service's API with the service's *predefined credential type*; you reuse the existing OAuth/API-key credential and get the full API.
- **Pros**: full power of n8n inside the tool; branching, error handling, sub-sub-workflows, native nodes, custom logic. Reusable across agents. Independently testable.
One thing to know: HTTP Request has its own HTTP-level timeout (default 5 minutes); bump `options.timeout` for slow endpoints. The agent tool itself has no timeout; the agent waits as long as the tool takes. Pointing it at, say, the Notion API (with the Notion predefined credential) lets the agent compose path, method, and body itself; covering operations the native node doesn't expose. Trade-off: the agent is now writing API requests, which is more error-prone and needs a capable model plus clear endpoint guidance in the description. That widens the blast radius; make sure the user understands.
- **External MCP servers**; any third-party or self-hosted MCP (GitHub, Linear, Notion, custom internal). One node exposes every tool that server offers.
- **n8n-hosted MCP**; a workflow on the same instance published with MCP access enabled. Same client node, pointed at an n8n MCP trigger URL. Lets one workflow serve many agents.
Pure inline computation (math, parsing, formatting). Its runtime contract is **string in / string out, no `$fromAI`, no `$helpers`** and is owned by the **n8n-code-tool** skill; read it before writing one. Rule of thumb: if you want `$fromAI()` in the code, you want `.toolWorkflow` instead.
- **paramName**; the name the model uses internally. snake_case or camelCase, be consistent.
- **description**; what value to produce. **Part of the prompt.** Be specific: format, range, example.
- **type**; `'string'` (default), `'number'`, `'boolean'`, `'json'`. Enforced; a wrong-typed value fails the call.
- **defaultValue**; used when the model omits the parameter.
It carries **JSON only**; it cannot carry binary (no base64, no file bytes), even through a non-AI binding. For binary, pass a storage key as a string and have the tool re-fetch (→ **n8n-binary-and-data**).
Treat `$fromAI` descriptions like JSDoc; the model reads them to figure out what to pass.
Not every parameter has to be `$fromAI`. Any parameter can be filled deterministically from workflow context, and **plumbed values are not model-selected inputs**; not in the tool schema, must still be validated server-side against authenticated context:
- **Identity**; `userId`, `customerId`, authenticated actor, tenant scope.
- **Authority limits**; refund caps, tier flags, allowed regions.
- **Correlation IDs**; `sessionId`, idempotency keys, trace IDs.
**Give the agent a button to push, not a steering wheel.** The strongest version is a sensitive tool with **zero `$fromAI` parameters**: a "Refund order" tool takes `orderId` from the trigger, `amount` from the fetched order record, `actor` from the session; all plumbed. The model only selects invocation, but deterministic inputs still require ownership, amount/currency, remaining balance and replay checks. Use an action-specific idempotency key, not a conversation ID. Pair with **HUMAN_REVIEW.md** for actions needing both deterministic params and sign-off.
**Bad names and descriptions cause bad selection; usually silently.** The model just doesn't call your tool, or calls a different one with garbage parameters. No error.
Do NOT use for generating new images from scratch; use Generate Image for that.
That description does work that would otherwise bloat the system prompt; which is exactly the point.
- **maxIterations.** Agents have a configurable tool-call cap (`options.maxIterations`), and the default is **low**. A multi-tool agent that chains calls hits it and surfaces "max iterations reached" or empty output. Raise it. Build a fallback; don't trust graceful recovery.
- **Tool-call cost.** Each call is at minimum one extra model round-trip. Frequently-called tools should return **concise** results; bloated returns burn input tokens fast.