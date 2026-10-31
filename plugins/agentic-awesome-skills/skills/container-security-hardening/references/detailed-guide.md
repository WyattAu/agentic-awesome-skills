- The tech stack (Node, Python, Go, Java; affects base image choice)
> Work through layers in order; hardening the image first gives the most leverage.
# ❌ AVOID: massive attack surface (~100–200 CVEs typical)
# ✅ BETTER: slim variants (glibc, smaller apt footprint)
# ✅ BEST: distroless (no shell, no package manager, built-in nonroot user)
# ✅ ALSO GREAT: Alpine (musl libc; verify app compatibility first)
# ✅ NO BASE USERSPACE PACKAGES: for fully static binaries only
### 1.2 Multi-Stage Build: Separate Build from Runtime
# ── Stage 2: Runtime: minimal, no build tools ────────────
USER nonroot:nonroot                # UID 65532; built into distroless
# For debian/ubuntu-based images: create dedicated user
USER appuser    # Switch before CMD/ENTRYPOINT; never run as root
# For distroless: nonroot (UID 65532) is already built in
# ❌ UNSAFE: tags are mutable; image can be silently overwritten (supply chain attack)
# ✅ SAFE: SHA256 digest is cryptographically immutable
# ❌ NEVER: secret in ENV or RUN; visible in `docker history` and layer cache
ARG API_KEY                         # Also unsafe; visible in build args history
# In the Dockerfile: use exec form (no shell interpretation)
# ENTRYPOINT /bin/sh -c "node..."  # ❌ shell form: spawns extra process
### 2.1 Trivy (Recommended: Fast, Comprehensive)
# CVE-2023-1234: only exploitable via X feature, not used in this app
# CVE-2023-5678: fix not yet available; tracked in issue #42
### 2.3 Hadolint: Dockerfile Linting
### 2.5 CI Integration (GitHub Actions: SHA-Pinned)
### 3.2 Linux Capabilities: What to Drop and Keep
| `CHOWN` | Change file ownership | No; set ownership at build time |
| `SETUID` / `SETGID` | Switch user identity | No; drop always |
| `SYS_ADMIN` | Broad privileged operations | No; most dangerous capability |
| `DAC_OVERRIDE` | Override file permissions | No; runs as correct user |
aa-genprof myapp   # Interactive; run app under aa-complain mode first
### 4.1 Sign Images with Cosign (Sigstore: Keyless)
# Sign after push: keyless via OIDC (no long-lived keys)
**GitHub Actions; Sign & Verify Pipeline:**
### 4.4 Admission Control: Block Unsigned/Unscanned Images
# Kyverno policy: require signed images before admission
### 5.3 NetworkPolicy: Zero-Trust Networking
### 5.4 RBAC: Least Privilege
# Create minimal role: never use wildcards
| Container needs `SYS_ADMIN` | Missing `--cap-drop` context | Investigate why; almost always avoidable |
- `references/base-image-comparison.md`; Size, CVE count, shell/pkg-manager trade-offs: distroless vs alpine vs slim vs scratch
- `references/seccomp-profile-template.json`; Experimental syscall template; audit and narrow before use
- `references/kubernetes-pod-security.md`; NetworkPolicy, RBAC, OPA/Kyverno policies, service account hardening, PSA
- `docker-expert`; General Docker usage, Compose orchestration, image optimization
- `gha-security-review`; Security audit of GitHub Actions workflows
- `github-actions-advanced`; CI pipeline patterns including scanner integration
- `kubernetes-architect`; Full Kubernetes architecture, not just security
- `api-security-best-practices`; Application-level security (injection, auth, OWASP)
- `k8s-security-policies`; Extended Kubernetes security policies