# HUNT-K8S: Kubernetes & Docker Security
- **K8s API anonymous cluster-admin**; `system:anonymous`/`system:unauthenticated` bound to a powerful role (classic misconfig: `system:anonymous` in a `ClusterRoleBinding` to `cluster-admin`) → full `kubectl`. Mere anonymous `200` is NOT this (see false-positive section).
- **Kubelet `10250` exec/run**; `/run` returns command output directly; `/exec` is a SPDY/WebSocket stream (see Phase 3). Either → RCE in any pod → steal that pod's SA token.
- **API-server-mediated kubelet RCE**; `/api/v1/nodes/<node>/proxy/run/...` reaches the kubelet *through* the API server using your (low-priv) token; if RBAC grants `nodes/proxy`, you get pod RCE without touching 10250 directly. Primary 2024-2026 vector.
- **etcd `2379` unauth**; every Secret (SA tokens, TLS keys, app creds) stored, often plaintext (unless `EncryptionConfiguration` is set) → full credential dump.
- **docker.sock exposure**; a socket-capable SSRF/RCE primitive reaching `/var/run/docker.sock` → may create containers under the daemon’s authority; rootless/user-namespace/admission controls affect host impact.
- **Container escape via runc**; Leaky Vessels (CVE-2024-21626): `WORKDIR`/`process.cwd` pointing at a leaked `/proc/self/fd/<n>` host FD → break out of an attacker-controlled image/exec to host root.
- **SA token abuse**; auto-mounted token at `/var/run/secrets/kubernetes.io/serviceaccount/token`; check its real grants with SelfSubjectRulesReview before claiming impact.
- **K8s Dashboard skip-login / token-less API**; full cluster management UI reachable unauthenticated.
- A `200` on `/api/v1/namespaces` does **not** mean cluster-admin. The API server returns `200` with an authorized full list (possibly empty because no resources exist); Kubernetes RBAC does not filter list results per object to *any* principal that can reach `list namespaces`; anonymous read on a few resources is common and low-impact. Confirm real privilege with **SelfSubjectRulesReview / SelfSubjectAccessReview**, then by accessing a pre-approved synthetic canary resource, without extracting production secrets.
- **10255 (read-only) vs 10250 (exec)** are constantly conflated. 10255 (HTTP, no auth) is info-disclosure only; it has `/pods`, `/stats`, `/metrics`, NO exec/run. 10250 (HTTPS) is where `/run` and `/exec` live. Do not report "kubelet RCE" off a 10255 hit.
- Use a **dedicated test namespace / test pod** when you have create rights; never exec into production workloads to "prove" RCE; list the pod and exec a read-only `id` in a pod you spun up if policy allows, or limit to a single non-destructive `id` and stop.
## Phase 1: Fingerprint & Port Discovery
# API server fingerprint: the /version endpoint is anonymous on most clusters
Note the `gitVersion`; it gates every CVE below.
## Phase 2: Kubernetes API Anonymous / Low-Priv Access
# 4. Only if access review says allowed: read only a designated synthetic test Secret to demonstrate the approved boundary
## Phase 3: Kubelet (Port 10250): `/run` First, `/exec` Done Right
# --- PRIMITIVE A: /run: returns command output DIRECTLY (no stream handling) ---
# --- PRIMITIVE B: /exec: SPDY/WebSocket stream, NOT a plain POST ---
# Option 2: raw: the POST returns a 302 to a stream path; -v to see Location, then
# Read-only kubelet 10255: INFO DISCLOSURE ONLY, no exec/run. Do not call this "RCE".
**CVE-2020-8558** (host-network trust): on affected kube-proxy, services bound to the node's `127.0.0.1` (incl. the read-only kubelet and other localhost-only services) become reachable from other pods/adjacent hosts via the node IP, defeating the localhost trust boundary; a lateral path to kubelet/etcd that were assumed loopback-only.
## Phase 4: API-Server-Mediated Kubelet RCE (`nodes/proxy`)
`nodes/proxy` in any bound role is effectively node-wide RCE. **CVE-2022-3294** (kube-apiserver node-address validation): an authenticated user could redirect the API server's proxy connection to an arbitrary host/IP it could reach (proxy-to-internal SSRF / node impersonation); relevant whenever you can influence node addresses or use the proxy subresource.
## Phase 4b: Ingress-NGINX "IngressNightmare" (CVE-2025-1974)
## Phase 5: etcd Unauth (Port 2379)
A recovered SA token from etcd → replay against the API server (Phase 6) to confirm grants. **False positive:** a `200` from etcd peer port `2380` or a TLS-required port returning a handshake error is not unauth client access; only a successful `range`/`get` with key data is.
## Phase 6: Service Account Token Abuse (Bound / Projected Tokens)
#          "exp" (projected token lifetime is configurable: a captured token may already be dead),
#          "kubernetes.io/serviceaccount" (pod/node binding: token dies with the pod).
## Phase 7: Docker Socket Exposure & runc Container Escape
- **CVE-2024-21626; "Leaky Vessels" (runc ≤ 1.1.11):** a leaked host file descriptor via `/proc/self/fd/<n>` lets a malicious image (`WORKDIR /proc/self/fd/N`) or `runc exec` cwd escape to the host filesystem → host RCE. Test only with an image you control on a build/registry surface where you can influence the Dockerfile.
## Phase 8: Dashboard, Admission, Helm/Tiller Remnants
# Kubernetes Dashboard: correct API base is /api/v1/... UNDER the dashboard service.
# Helm 2 / Tiller remnant: gRPC on 44134, historically NO auth → full cluster as Tiller's SA
# Validating/Mutating admission webhooks: enumerate to find bypassable policy or SSRF-able webhook URLs
| Projected SA token (aud matches) | API access scoped to its real RBAC | Depends on RBAC; verify first |
- **`/exec` plain-POST returns 302, not output.** Seeing no body is NOT "patched"; follow the stream (kubeletctl/websocat) before concluding either way.
- **etcd plaintext assumption.** If `EncryptionConfiguration` is enabled, only resources covered by an effective encryption provider configuration are encrypted; identity providers, migration and historical data must be checked; don't claim "plaintext secrets" without showing decoded bytes.
- [ ] **Kubelet:** literal `id`/`hostname` output returned from 10250 `/run`, or a completed `/exec` stream; not a bare 302.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.