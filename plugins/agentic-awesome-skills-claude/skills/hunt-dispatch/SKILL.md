- **authorized and scope-bounded.** invoking `/hunt` does not itself assert authorization. Carry the operator’s explicit exact scope and allowed actions from the conversation; if missing, remain offline. testing stays inside it. an out-of-scope host ends the run; it does not
## step 0: 404 baseline (ALL modes, mandatory, before any enumeration)
re-derive the baseline per host; it differs across an estate. one engagement saw
## step 1: fingerprint (red team only)
real targets almost always return multiple signals at once; e.g. a single host
        (own the auth boundary; highest blast radius if compromised)
- CDN banner alone (Cloudflare/Akamai/Fastly) is **not** a platform match; it
## step 2: load skill set
validated.** `/hunt` only prompts for and stores creds (commands/hunt.md); it
1. **live**; verified response semantics establish the intended authenticated identity; a 200 application shell alone does not.
2. **role/privilege**; the `/api/me` (or equivalent) body shows the expected
3. **not MFA-gated**; login did not stop at a 2fa/step-up challenge. if it did,
   you hold creds but **not** a session; default to least capability and confirm
if the preflight fails, do **not** silently continue as blackbox; surface
## step 3: taxonomy print (once, at session start)
   "*.target.com"; the explicit list. a subagent cannot infer the boundary.
   `generate`, `process`. only then allow read-shaped names. order matters -
authorized list; one the parent prompt had named only for a DNS check. both were
## step 4: return control to /hunt
after taxonomy print, return a plan to the authorized host/operator. No `/hunt` orchestrator or delegation engine is bundled; delegation requires host/user authority and exact scope/budget propagation. do not run probes here; this skill only loads context.
- **`bb-methodology`**; When PART 0 mode confirmation completes. Workflow primitive: `bb-methodology` confirms engagement type (red team vs WAPT vs bug bounty); the answer feeds directly into this skill's `mode=redteam` / `mode=wapt` invocation.
- **`redteam-mindset`** + **`mid-engagement-ir-detection`**; When `mode=redteam` is loaded. Workflow primitive: these are the always-on skills loaded first by step 2 of the redteam flow before any platform skill or hunt-* skill.
- **`okta-attack`** / **`m365-entra-attack`** / **`enterprise-vpn-attack`** / **`vmware-vcenter-attack`** / **`cloud-iam-deep`** / **`supply-chain-attack-recon`** / **`apk-redteam-pipeline`**; When fingerprint signals match. Workflow primitive: step 1's curl fingerprint scan against `recon/<target>/live-hosts.txt` maps banner / domain signals to one or more of these platform skills.
- **`hunt-rce`** / **`hunt-sqli`** / **`hunt-ssrf`** / **`hunt-ato`** / **all other hunt-* skills`**; When the mode-specific skill set is being printed. Workflow primitive: this skill is the loader; it names the hunt-* skills but does not run probes; actual hunting happens after step 4 returns control to `/hunt`.
- **`report-writing`** vs **`redteam-report-template`**; When the taxonomy print specifies the report format. Workflow primitive: `mode=wapt` ends with `report-writing` as the deliverable format; `mode=redteam` ends with `redteam-report-template` instead.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.