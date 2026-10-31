wrong data and a write lands in the wrong place; usually with **no error** (the one exception is an
   (and before destructive workflow edits), confirm `current` is the instance you intend; primary
   write a secret because this session never picked a target itself. Comply; run `switch` here to
| `INVALID_CONTEXT` | server-side metadata missing | A server bug, not your input; report it. |
A separate, higher-stakes error. It is **not** returned by `n8n_instances`; it's returned by the
 ; you should not need to re-switch before every call.
- **Reads and non-credential writes** route to the currently-selected instance, silently; a
  falls back to your **default** instance; no error. So default's data appearing where you expected
| `INSTANCE_AMBIGUOUS` on a credential create/update/delete | This session never switched itself; the system won't guess which instance to write the secret to | Run `n8n_instances({mode:"switch", name:"<target>"})` on this session (the error names `lastSelected` and `default`; pick the one you want), then retry the write. Never retry blindly. |
| `NOT_FOUND` for a workflow/datatable/credential you **know exists** | You're pointed at the wrong instance; **not** that it was deleted | `n8n_instances({mode:"list"})` → check `current`. If it's not your target, `switch` and retry. **Do not recreate the object.** |
instance**. The server protects the **ambiguous** case automatically; if this session never picked
- **Verify `current` immediately before** `n8n_manage_credentials` create/update/delete; call
- **On `INSTANCE_AMBIGUOUS`**, switch on this session to confirm the target, then retry; don't
  secret, but a read off the wrong instance returns the wrong schema or list; so still verify
2. switch → B   (its own call; never batched with the create below)
- Change target: `n8n_instances({mode:"switch", name:"<name>"})`; its own turn, then operate
- **n8n-mcp-tools-expert**; owns `n8n_manage_credentials` (CRUD + `getSchema`) and the rule that
- **using-n8n-mcp-skills**; the router; consult it for which skill owns a given build step.