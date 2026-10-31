# HUNT-GRPC: gRPC Security
gRPC reflection enabled = full service catalog enumeration without source code. The highest-value gRPC bugs come from the architectural assumption that a service is "internal"; auth is enforced at the edge proxy, and the backend trusts any caller that reaches it. Once you reach the backend directly (exposed port, SSRF, proxy bypass), that trust collapses.
- **Reflection enabled in production**; `grpc.reflection.v1alpha.ServerReflection` / `grpc.reflection.v1.ServerReflection` may list registered services/descriptors, subject to reflection policy and registration. Enumeration enabler, not a vuln on its own (see Validation).
- **Missing auth on internal service**; a service designed for east-west microservice traffic exposed externally with no mTLS and no per-method authorization → call privileged methods directly.
- **Edge-auth-only / metadata-stripping**; proxy authenticates the user but the backend re-trusts proxy-injected headers (`x-user-id`, `x-tenant-id`, `x-forwarded-*`); if you reach the backend or can inject those headers via the proxy, you impersonate any tenant.
- **Plaintext gRPC**; gRPC h2c (cleartext HTTP/2) on a non-standard port → credential/metadata interception.
- **HTTP/2 Rapid Reset DoS (CVE-2023-44487)**; interleaved HEADERS + immediate RST_STREAM frames bypass `MAX_CONCURRENT_STREAMS` accounting → resource exhaustion. **DoS is in scope on almost no program; get explicit written authorization before sending a single burst.**
## Phase 1: Fingerprint & Port Discovery
# ALPN must negotiate h2: native gRPC normally requires HTTP/2; gRPC-Web/Connect can use other HTTP versions
# trailer (12 = UNIMPLEMENTED) even when the path is wrong: strong signal it's gRPC.
`grpc-status` trailer present ⇒ a gRPC server (or grpc-gateway/Envoy) is behind that port. `UNIMPLEMENTED` on a random path is normal and only confirms the transport; not a finding.
## Phase 2: Service Enumeration via Reflection
## Phase 3: Call Methods Without Authentication (authz testing)
## Phase 4: Authentication / Trust-Boundary Bypass
The metadata-stripping bug (b) is the gRPC-specific crown jewel: confirm it by sending the spoofed header **directly to the backend port** AND, separately, **through the public proxy**; if the proxy forwards your `x-user-id` unchanged to the backend, it is exploitable for real users, not just on the bypassed port.
## Phase 5: Proto File / Schema Discovery
## Phase 6: gRPC-Web / grpc-gateway / JSON-Transcoding Attacks
#     map predictably: hit them over plain HTTP/JSON (no gRPC client needed):
## Phase 7: HTTP/2 Rapid Reset DoS (CVE-2023-44487)
The attack is NOT a load test. It opens streams (HEADERS) and immediately cancels them (RST_STREAM) before the server finishes, so each cancelled stream frees a `MAX_CONCURRENT_STREAMS` slot instantly while the server still spends work on it; the client races far ahead of the concurrency cap. `h2load`/`ghz` are throughput benchmarkers; **they have no rapid-reset mode and never interleave HEADERS+immediate-RST_STREAM, so they cannot test this.**
#   NOTE: ghz and h2load are LOAD benchmarkers, NOT rapid-reset testers: do not
| Admin method, no auth | Call privileged RPCs (`DeleteUser`, `GetConfig`) | Data manipulation / system access; Critical |
| Proxy forwards `x-user-id`/`x-tenant-id` unstripped | Spoof identity metadata → cross-tenant impersonation | Tenant isolation bypass; Critical |
| IDOR via enumerable id field | Iterate `user_id` over `GetUser` | Mass PII exfil; High |
| grpc-gateway / gRPC-Web route re-exposes internal RPC | Hit transcoded REST/JSON path unauth | Externally-reachable authz bypass; High/Critical |
| Plaintext h2c on internal port | MITM / sniff metadata (bearer tokens) | Credential capture; High |
| `.proto` leak (repo/swagger) | `-protoset` to drive reflection-off target | Unlocks Phases 3–4; Low alone, High as enabler |
## Validation: false-positive discipline
2. **Reflection / health endpoints are often intentionally public.** `grpc.reflection.*` and `grpc.health.v1.Health` being reachable is, by itself, an observation that may be intentional and non-reportable; many vendors ship reflection on by design. Do NOT report it as "missing auth" unless it leaks a non-public service catalog. The finding is the *sensitive* service you can then call without auth, proven in Phase 3.
4. **Proxy-vs-backend reachability.** A bug reachable only by hitting an internal `:50051` you found via SSRF/port-scan is real but its severity depends on reachability. State explicitly how an external attacker reaches it (exposed port, SSRF egress, proxy passthrough). For metadata-spoofing, prove the PUBLIC proxy forwards the spoofed header; not just the bypassed backend port.
6. **DoS is authorization-gated and version-verifiable.** Never submit CVE-2023-44487 off a benchmark "slowdown." Use owner-confirmed build/configuration evidence or a controlled isolated test ONLY under explicit written authorization with an agreed window; then stop immediately. A slow response is not proof.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.