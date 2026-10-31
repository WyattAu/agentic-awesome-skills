Conventions for webhook API response bodies; both success and error. The goal is **predictability**: a caller, a dashboard, or a retry loop should be able to branch on your response without guessing. Pick a shape and hold it across every endpoint on the instance.
- Top-level keys; envelope vs bare, presence of `error`/`message`/`request_id`.
If results are sparse, mixed, or you can't tell whether a convention exists; **ask the user.** "Endpoints A and B use shape X, C uses Y; which is house style?" saves a future migration. Don't invent a domain prefix or envelope from nothing.
- `message` is the human version; safe to log, safe to show users *after* sanitization.
- No `ok: false` flag; the HTTP status code already separates success from failure.
## `responseCode` defaults to 200: set it on every error branch
**Every `Respond to Webhook` node defaults `responseCode` to 200**; including the ones you wired to error paths. An error branch that returns 200 with `{ "error": "..." }` looks like success to the caller's HTTP client, so their error handling (which keys off the status code) **never fires**. They process your error body as if it were data.
So: set `responseCode` **explicitly** on every Respond node; not just the success one. (This trap is also documented in **n8n-node-configuration** NODE_FAMILY_GOTCHAS.md, "Webhook / Respond to Webhook".) A workflow can have many Respond nodes, one per response shape; n8n returns whichever fires first.
For paths that differ only by number, set it with an expression instead of fanning out to N nodes; see **API_WORKFLOWS.md**, "5xx: differentiate the body".
- **2xx**; success. 200 sync, 202 "accepted, processing".
- **4xx**; caller's fault. 400 bad input, 401 no auth, 403 not allowed, 404 not found, 409 conflict, 429 rate limited.
- **5xx**; your fault. 500 unexpected internal, 502 upstream broken, 503 temporarily down, 504 upstream timeout.
- Aggregated error rates segment by class; collapse everything to 500 and you lose that.
**4xx; caller's fault**
**5xx; your fault**
| **Stack traces**; `{ "stack": "Error at line 42 of /opt/..." }` | Reveals paths, versions, library names. A gift to attackers, useless to callers. |
| **Upstream errors verbatim**; `{ "details": "<raw upstream body>" }` | Upstream may embed *their* tokens and PII. Surface "upstream service failed" + a request id; details go to your logs. |
| **SQL queries**; `{ "query": "SELECT * FROM users WHERE ..." }` | Exposes schema and access patterns. |
| **Tokens / credentials / auth values** | Even innocuous-looking `headers`, `config`, or `request` fields can carry token values. Audit error bodies; leaks are easier than you'd expect. |
- **With `respondWith: "json"`, pass the object, not a stringified string.** If you hand it `JSON.stringify(obj)` it serializes that string *again* and you get a double-encoded body. Either use `respondWith: "json"` with an object expression (`={{ { error: 'x' } }}`), or keep `JSON.stringify(...)` and let the node treat it as the already-final body; pick one and be consistent. (See **n8n-node-configuration** NODE_FAMILY_GOTCHAS.md.)