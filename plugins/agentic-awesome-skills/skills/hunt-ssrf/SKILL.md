- **Kubernetes/orchestration platforms**; aggregated API servers, metrics-server, kubelet endpoints expose privileged cluster operations
- **Internal developer tooling**; CI/CD, workflow orchestration (Flyte, Argo), admin panels not exposed externally
- **Link preview / URL fetching features**; Reddit-style preview APIs, Slack-style unfurling, media processors
- **Dataset/file import pipelines**; anything that fetches remote URLs on behalf of a user
- **Enterprise self-hosted software** (GitHub Enterprise, GitLab); SSRF frequently chains to RCE via internal services
- The server **echoing your URL back in an error message**. Example: `"The Web application at http://evil.example.com/x could not be found"`; this is the server formatting your input into an error string, NOT making an outbound HTTP request. The error came from string formatting, not from network failure.
   subdomain back to you; **verify that before relying on it.** Burp's
BAD  ; four candidate fields, one payload, fired in one batch
GOOD ; fresh payload per field, one request each, poll between
and it belongs in the report; it is what lets the client fix the right field
so that was the obvious suspect; but `apiUrl` was inert and **`url` was the live
### Blind vs full-read: establish which before scoring
- **Full-read** (upstream body in the response): substantially higher severity -
- **Kubernetes**; any public-facing aggregated API, metrics endpoints
- **GCP**; any service fetching URLs that runs on Compute Engine/GKE
- **Headless browsers** (Puppeteer, PhantomJS) used for screenshots/PDF; extremely high value
- **XML/DSPL/CSV import features**; XXE-style SSRF vector
2. **Set up an out-of-band detection server** using Burp Collaborator, interactsh, or `https://canarytokens.org`; you need a unique per-test DNS/HTTP callback domain.
6. **Check for redirect-based SSRF**; if the endpoint validates the initial URL but follows 30x redirects, host a redirect server pointing to internal addresses. Kubernetes report (Report 3) was specifically triggered by hijacked API servers returning 30x responses.
9. **Chain findings**; if you have SSRF to internal services, look for:
1. **"The user said it was safe"**; Developers trust user-supplied URLs for fetching remote resources (link previews, thumbnails, webhooks) without validating the destination. The feature is legitimate; the missing validation is the bug.
2. **Allowlist bypass via redirects**; Developers validate the initial URL against an allowlist but configure HTTP clients to follow redirects automatically. An attacker's server on the allowlist redirects to an internal address.
3. **Aggregated/proxy API trust**; Kubernetes-style architectures where an API aggregation layer blindly proxies 30x responses from registered extension servers. Compromising a single extension server gives SSRF into the core API.
4. **Server-side rendering without sandboxing**; Headless browser features (PDF generation, link preview screenshots) execute attacker-controlled JavaScript in a network-privileged context with access to metadata services.
5. **XML/DSPL/file parsers fetching external entities**; Import features that parse structured files (XML, DSPL, CSV with remote schemas) fetch attacker-controlled URLs, often with no URL validation at all.
6. **Internal hostname leakage via response differences**; Services return different error messages, timing, or response sizes for internal vs. external hosts, enabling blind enumeration even when content isn't returned.
7. **Metadata reachability**; AWS IMDSv2 requires a session-token exchange; GCP normally requires `Metadata-Flavor: Google`. URL-only SSRF does not automatically satisfy these constraints or imply credential access.
- [Disclosed Report Citations (Backfill +6; 2018-2024)](references/details.md)
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.