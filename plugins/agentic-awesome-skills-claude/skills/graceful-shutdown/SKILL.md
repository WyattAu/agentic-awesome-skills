    console.log("Server closed; no new connections accepted");
    console.error("Drain timeout reached; forcing exit");
// Health endpoints; registered BEFORE the drain-rejection middleware so it
  console.log(`${signal} received; draining ${activeRequests} active requests`);
    console.error("Forced exit; drain timeout exceeded");
    console.log("No active requests; exiting cleanly");
    console.log("All requests drained; exiting cleanly");
- Always set a drain timeout shorter than the orchestrator's kill timeout (`terminationGracePeriodSeconds` in Kubernetes defaults to 30s; use 25s for your drain)
- Test shutdown behavior explicitly; simulate SIGTERM in integration tests and verify no requests are dropped
  **Solution:** Track request completion with both `finish` and `close` events (Node.js) or equivalent. If a client aborts the connection, `finish` may never fire; `close` will. Use a once guard to prevent double-decrementing the counter.
  **Solution:** Flip the readiness probe to 503 immediately on signal receipt; before starting to drain.
  **Solution:** Guard with a `isShuttingDown` flag; ignore the second signal.
- WebSocket and SSE connections require application-level close frames before severing; `server.close()` alone won't gracefully end them.
- Some cloud platforms (Heroku, Railway) send SIGTERM with very short grace periods (10-30s); adjust drain timeouts accordingly.
- `@api-rate-limit-handler`; Resilient retry and backoff for outbound requests
- `@circuit-breaker`; When to stop retrying entirely and fail fast
- `@error-handling`; Structured error handling patterns