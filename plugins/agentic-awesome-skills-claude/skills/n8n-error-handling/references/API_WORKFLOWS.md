When a workflow is an HTTP API; a Webhook trigger that ends at a `Respond to Webhook`; error handling stops being optional. The caller is a machine waiting on a response, and the failure modes are unforgiving: a hanging branch becomes a timeout, a wrong status code breaks the caller's error handling, a leaked stack trace becomes a security finding.
The non-negotiable: **every path ends at a Respond node.** Success, validation failure, execution failure; all of them. A path that doesn't reach a Respond is a hanging branch, and a hanging branch is a caller timeout.
Set `responseMode: "responseNode"` on the Webhook trigger; without it the trigger acknowledges immediately (`onReceived`) and the caller never sees your computed response. (See **n8n-node-configuration** NODE_FAMILY_GOTCHAS.md for the Webhook/Respond traps.)
// Error paths; both fan in to one responder
2. **Both nodes have `onError` set.** Miss it on either and that node's failure halts the workflow instead of routing; and the caller times out.
Always set `Content-Type: application/json` explicitly; the default depends on the body shape and isn't reliable.
That's not over-engineering; it's the difference between the caller getting an actionable `validation_error` and getting a generic 500 they can't act on.
The validator node assigns one object field, `result`, computed by the expression below. The expression is **schema-specific**; edit the `REQUIRED_SCHEMA` constant and the per-field checks for your endpoint. The *output keys* are a contract the Respond node consumes; don't rename them.
Echoing the schema back lets the caller; or an LLM driving the call; self-correct.
Inside a JSON `responseBody`/`value` string, a regex like `\S` in the `REQUIRED_SCHEMA` literal needs **four** backslashes (`^\\\\S+...`) because it survives two layers of escaping; JSON string → JS string. The regex literal *executed* inside the IIFE (`/^\\S+@\\S+\\.\\S+$/`) needs only two per `\S`. If your email validation silently never matches, this is why.
- **Caller-supplied**; read an `X-Request-ID` header, pass it through. Better for tracing across systems.
- **Generated**; use `{{ $execution.id }}` or a UUID. Easier.
Don't conflate this with the `job_id` an async (202) endpoint returns; that's how the caller polls for work later, not a correlation field.
It has its own gotchas (idempotency, callback retries, status tracking); build it deliberately. The `job_id` is intrinsic (it's how the work is found later), distinct from the optional `request_id`.
1. **Test the success path** with `n8n_test_workflow`. Confirm shape and code. **API workflows almost always have side effects (DB writes, third-party calls, comms); ask the user before running a test that triggers them.**
2. **Trigger an error path**; feed input that breaks a processing node, run, confirm the error Respond fires with the right code and body.