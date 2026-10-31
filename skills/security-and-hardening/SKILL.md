Security-first development practices for web applications. Treat every external input as hostile, every secret as sacred, and every authorization check as mandatory. Security isn't a phase; it's a constraint on every line of code that touches user data, authentication, or external systems.
3. **Run STRIDE over each boundary**; a quick lens, not a ceremony:
4. **Write abuse cases next to use cases.** For each feature, ask "how would I misuse this?"; then make that your first test.
- **Parameterize all database queries**; never concatenate user input into SQL
// CORS; restrict to known origins
Any time the server fetches a URL the user influenced; webhooks, "import from URL", image proxies, link previews; an attacker can aim it at internal services (cloud metadata, `localhost`, private IPs).
**Caveat; this still has a TOCTOU gap.** `fetch` resolves DNS again after the check, so an attacker using a short-TTL record can rebind to an internal IP between validation and connection. For any surface exposed to untrusted DNS, use a reviewed transport that connects only to approved resolved addresses while preserving TLS hostname validation, and enforce network egress restrictions. Bound response bytes/time/concurrency and revalidate every permitted redirect. Named third-party filtering packages require current compatibility/security review; their names are not a guarantee.
- **Commit the lockfile** and install with `npm ci` (not `npm install`) in CI; reproducible builds, no silent version drift.
- **Review new dependencies before adding them**; maintenance, download counts, and whether they truly earn their place. Every dependency is attack surface (OWASP **A06: Vulnerable Components**, **LLM03: Supply Chain**).
- **Be wary of `postinstall` scripts** in unfamiliar packages; they run arbitrary code at install time.
- **Watch for typosquats**; `cross-env` vs `crossenv`, `react-dom` vs `reactdom`.
**If a secret is ever committed, rotate it.** Deleting the line or rewriting history is not enough; assume it's compromised the moment it reaches a remote. Revoke and reissue the key first; coordinate any history rewrite separately with the owner and collaborators. Do not rewrite shared history automatically.
If your app calls an LLM; chatbots, summarizers, agents, RAG; it inherits a new attack surface. Map it to the [OWASP Top 10 for LLM Applications (2025)](https://genai.owasp.org/llm-top-10/):
- **Assume prompts can be hijacked (LLM01: Prompt Injection).** Untrusted text in the context window; a user message, a fetched web page, a PDF; can carry instructions. The system prompt is not a security boundary; enforce permissions in code, not in the prompt.
// GOOD: model output is data; parse defensively, then validate, then encode