When the endpoint takes the target URL **wrapped/encoded**; a base64 blob in the path, a
double-encoded param, a JSON-nested value; the front-end WAF/allowlist inspects the *raw*
Always test whether the parameter is decoded once, twice, or base64'd before the fetch; the number of decode passes is the bug.
path segments the edge left intact; inject `;/`, `/../`, or matrix-param segments so the edge
   - "The server makes a request" alone is insufficient; show *where* it goes and *what comes back*.
   - If the answer is only "the server fetches my URL," severity is low; quantify the actual reachable blast radius.
   - If reproduction requires specific timing, tokens, or luck; resolve the flakiness before submitting.
An attacker who could register a Kubernetes API extension server (metrics-server equivalent) returned `302 Location: http://127.0.0.1:6443/api/v1/secrets` responses to the aggregation layer. Because the aggregation proxy followed redirects automatically without re-validating the destination against the internal network blocklist, the redirect caused the aggregation layer itself (running with elevated cluster credentials) to fetch internal Kubernetes API secrets and return them in the response. This effectively allowed an attacker with limited API registration privileges to escalate to full cluster secret read access; a critical privilege escalation via SSRF chained through trusted infrastructure components.
## Disclosed Report Citations (Backfill +6: 2018-2024)
3. **HackerOne; SSRF in Analytics Reports (PDF generator → AWS metadata)** ([H1 #2262382](https://hackerone.com/reports/2262382) · [Writeup](https://osintteam.blog/25-000-ssrf-in-hackerones-analytics-reports-b9a5b3aa3d6e))
    - Year: 2023; **$25,000** (CVSS 10.0 Critical)
4. **Shopify Exchange; SSRF in screenshot service → GCP metadata → container root** ([H1 #341876](https://hackerone.com/reports/341876))
    - Year: 2018; **$25,000** (canonical headless-browser → metadata)
5. **Concrete CMS; SSRF mitigation bypass via DNS rebinding → AWS IAM keys** ([H1 #1369312](https://hackerone.com/reports/1369312))
    - Year: 2021; fixed in 8.5.7 / 9.0.1
6. **Yahoo Mail; Blind SSRF → Gopher → Redis RCE** ([Writeup](https://sirleeroyjenkins.medium.com/just-gopher-it-escalating-a-blind-ssrf-to-rce-for-15k-f5329a974530))
    - Payload: blind SSRF in Yahoo Mail backend reached via `gopher://internal-redis:6379/_*1%0d%0a$8%0d%0aflushall...SET stuff /var/spool/cron/root...BGSAVE`; wrote a cron via Redis to get command execution
    - Year: 2020; **$15,000**
7. **Reddit Matrix; Blind SSRF in `preview_url` API** ([H1 #1960765](https://hackerone.com/reports/1960765))
    - Payload: `GET https://matrix.redditspace.com/_matrix/media/r0/preview_url/?url=http://10.0.0.0:80/`; varied internal IPs/ports; service names and IPs leaked through response differences before the fix
    - Year: 2023; **$6,000**
8. **Azure DevOps; SSRF in Service Hooks + DNS rebinding bypass in endpointproxy** ([Binary Security writeup](https://www.binarysecurity.no/posts/2025/01/finding-ssrfs-in-devops))
    - Year: 2023-2024; **$15,000 total** across 3 reports
- **`cloud-iam-deep`**; SSRF is the canonical entry to cloud metadata service. Chain primitive: SSRF → IMDSv1 token theft → `cloud-iam-deep` privilege escalation reaches `iam:CreateUser` / `sts:AssumeRole` on cross-account roles.
- **`hunt-llm-ai`**; LLMs with fetch_url tools become SSRF proxies bypassing network egress controls. Chain primitive: LLM tool-use (fetch_url) + SSRF → attacker URL exfils chat history and IMDS token from the LLM container.
- **`hunt-rce`**; Internal Redis/Memcached authentication, protected-mode, network reachability and supported URL schemes depend on configuration. Chain primitive: SSRF + Gopher → internal Redis `CONFIG SET dir` + RCE via cron / SSH authorized_keys write.
- **`hunt-cloud-misconfig`**; Internal-only buckets/APIs become reachable through SSRF egress. Chain primitive: SSRF + DNS rebinding → SSRF-protected-endpoint bypass → internal /admin or private S3 bucket read.
- **`security-arsenal`**; Load the SSRF IP Bypass Table (11 techniques: decimal IP, IPv6 mapped, octal, suffix dot, DNS rebinding, redirect chain, etc.) before testing filters.
- **`triage-validation`**; Apply the OOB-Or-It-Didn't-Happen gate: every blind SSRF claim requires a Burp Collaborator hit with a unique marker before report submission.