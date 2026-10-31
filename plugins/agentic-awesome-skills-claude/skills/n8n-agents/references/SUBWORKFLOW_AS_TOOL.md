For the sub-workflow primitive itself (Execute Workflow Trigger inputs/outputs, stateless design, naming, search-before-build), see **n8n-subworkflows**; this reference only covers the *agent-tool* angle.
### 1. The sub-workflow side: an Execute Workflow Trigger with typed inputs
Each declared input becomes a parameter the caller can fill. **The trigger must be in "Define Below" mode (typed fields), not passthrough**; passthrough has no schema, so the agent has nothing to fill via `$fromAI`. Two exceptions: (a) the sub-workflow needs binary (it can't be an agent tool directly; pre-stage to storage and pass storage keys as typed string fields, see **n8n-binary-and-data**), or (b) the tool takes no inputs at all (passthrough is the only option, and the tool's only decision is whether to invoke).
### 2. The Tool Workflow side: points at the sub-workflow, binds params
- **Agent-filled**: `={{ $fromAI('paramName', 'description', 'string') }}`; the agent decides.
- **Plumbed**: `={{ $('SourceNode').first().json.field }}`; your workflow fills it.
The `sessionId` line is critical: it is **NOT** an agent decision. Plumb it from the trigger so memory and session-keyed work stay consistent. **Never put `sessionId` behind `$fromAI`**; the agent will fabricate a UUID.
The agent sees the tool's **name** (the Tool Workflow node's name) and **description** (a parameter on the node); both follow the **TOOLS.md** rules: specific, API-doc style, treated as prompt.
The agent picks the mode by what it puts in `imageName`. Two near-identical tools would have made selection harder; collapse them.
Don't sometimes return `{ url, key }` and other times `{ result: { url, key } }`. The output shape is a contract every caller depends on; agents read it as part of the prompt, deterministic callers wire downstream nodes to specific paths. Drift breaks callers silently.
- **Simple one-node wrappers**; "call this endpoint and return" is shorter as an HTTP Request Tool.
- **One-off code-only logic specific to this agent**; a few lines of pure JS/Python that exist nowhere else work fine as a Custom Code Tool (`.toolCode`, see **n8n-code-tool**). Decision rule: reusable business logic → sub-workflow; one-off agent-specific transform → Code Tool.
- **Capabilities that already exist as native tool nodes**; don't wrap `slackTool` in a sub-workflow.