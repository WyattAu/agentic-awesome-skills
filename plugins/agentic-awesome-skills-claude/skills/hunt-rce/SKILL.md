Traditional web forms use `Content-Type: application/x-www-form-urlencoded`. If you send a JSON body (`{"host":"127.0.0.1;id"}`) to a form endpoint, the server reads `request.form['host']` and gets nothing; the app executes normally with no injection, potentially returning a plausible 200 response. You get a false negative with no indication anything went wrong.
**Candidate evidence:** Unexpected OS command output (`uid=N(username) gid=...`) in the response body confirms code execution. The output may be HTML-wrapped; that still counts. If the response is otherwise normal (200, expected content) with the command output appended or embedded, use a unique non-sensitive marker and a control request to rule out reflection, static text or a scanner intermediary.
- **Enterprise server products** (GitHub Enterprise Server, self-hosted GitLab); privilege escalation chains from low-privileged console roles to root SSH access consistently pay critical/high
- **Supply chain / package registries**; dependency confusion attacks against npm, PyPI, etc. hit critical severity across every major program
- **Cloud-native infrastructure**; exposed Kubernetes API servers, ingress controllers, and misconfiqured CI/CD pipelines
- **Mobile app backends and OAuth flows**; where server-side processing of attacker-controlled data meets execution contexts
- **Admin/management consoles**; template injection in configuration panels reaches root with a single payload
- Proof-of-concept is unambiguous; shell output is undeniable
X-Powered-By: Express          # Node.js; npm dependency surface
Server: nginx (ingress-nginx)  # Kubernetes ingress; path field injection
Content-Type: application/yaml # YAML parsers (SnakeYAML, Psych); deserialization
X-GitHub-Enterprise-Version    # GHAS; nomad template, collectd, syslog-ng injection
2. **Enumerate admin/management interfaces.** Crawl for `/management-console`, `/admin`, `/_internal`, `/setup`, `/config`. These surfaces are lower-auth and higher-privilege; the GHES cluster produced 6 separate RCEs from one console role.
5. **Hunt dependency confusion.** For every npm/pip/gem internal package name visible in JS bundles, error messages, or `package.json` in public repos; do not register target package names or publish install hooks without explicit supply-chain testing authorization. Prefer an isolated registry/build reproduction.
# Generic polyglot: works across Jinja2, Twig, Freemarker, Pebble, Velocity
# Note: --path-as-is is REQUIRED: curl normalizes %2e by default
**Triage discipline note:** when the same path-traversal primitive works on multiple aliases but only one is CGI-enabled, the **maximum** impact is the severity; not the average. A "file read" finding on `/icons/` should always be escalated by re-probing `/cgi-bin/` (and any other alias visible from `<Directory>` blocks in the server-info disclosure or response patterns). See `triage-validation` Pre-Severity Gate.
Java CLIs built on the `args4j` library default to `expandAtFiles=true`, which expands `@filename` arguments by reading the file and treating each line as a separate command argument. When such a CLI is exposed over HTTP (Jenkins CLI is the canonical case), the server-side error message echoes failed arguments back; turning argument echoing into an arbitrary file-read primitive. Jenkins 2.441/LTS 2.426.2 and earlier are affected; fixed in 2.442/LTS 2.426.3/2.440.1. Overall/Read permits full-file reads; without it only limited lines may be exposed, depending on available commands. Do not assume anonymous permissions.
- `/var/jenkins_home/secret.key`; master encryption key for stored credentials
- `/var/jenkins_home/secrets/master.key`; derives the encryption key
- `/var/jenkins_home/credentials.xml`; credential store (encrypted with secret.key; pair with offline decrypt tools)
- `/var/jenkins_home/users/*/config.xml`; per-user API tokens (often unencrypted)
- `/var/jenkins_home/jobs/*/config.xml`; pipeline configs that may inline AWS keys, SSH keys, registry tokens
Ingress/nginx annotation values, Kubernetes spec fields, and webhook URLs are treated as opaque strings; but the downstream processor (nginx config generator, regex engine) interprets them as code.
# Blind: use DNS callback instead of output
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.