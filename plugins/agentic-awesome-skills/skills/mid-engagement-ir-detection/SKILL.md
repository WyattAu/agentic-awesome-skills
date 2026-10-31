- Engagement is "assume breach" or "white box"; client knows you're testing
3. **Classification signals** (WAF rule deployment vs code fix; different remediation depth)
## The discipline: capture before, diff after
### Observation 1: Mid-engagement WAF rule deployment
### Observation 2: Possible concurrent activity or authentication policy change
### Observation 3: Detection-induced rate limiting / IP blocks
Subject: Engagement traffic detected; IP <X> rate-limited at <timestamp>
## State diff: the key technique
### 1. `engagement_log/baseline.json`: captured at session start
### 2. `engagement_log/journal.jsonl`: append-only test log
### 3. `engagement_log/state_changes.jsonl`: observed deltas
{"ts_observed":"2026-05-08T14:30:00","change_type":"baseline_time_shift","target":"https://<employee-app-host>/<app>/login.php","baseline_pre_ms":24412,"baseline_post_ms":90403,"interpretation":"likely WAF rule deployed","actions_taken":["tested WAF-evasion variants; no signal restoration","documented as IR-mitigation finding"]}
## Tooling: automated state-change detection
# Bash watcher: runs every 5 min during engagement, alerts on shifts
- **Identified the depth question** (WAF vs code; different remediation cost)
- **"Don't include the locked accounts list; they'll think we caused it."** No actor attribution follows from a response code. Share minimized account evidence only with the authorized SOC and state uncertainty.
- `redteam-mindset`; broader discipline framework; this skill is a specific application
- `m365-entra-attack`; specific case for M365 / Smart Lockout differential
- `evidence-hygiene`; how to capture and redact PoC evidence properly
- `report-writing`; finding template for IR observations
- `bb-methodology`; note that this skill is INAPPROPRIATE for bug bounty (no real-time IR there)
- **`redteam-mindset`**; This skill is a specific application of the broader red-team discipline. Engagement flow: `redteam-mindset` loaded at engagement start → baseline-capture habit built in → when response patterns shift mid-test, `mid-engagement-ir-detection` activates to capture the SOC-patch state as a NEW finding (defensive-action observed = client capability metric, not "the bug got fixed so we lose the finding").
- **`evidence-hygiene`**; Mid-engagement IR detection produces TWO states (pre-patch and post-patch); both need disciplined evidence capture or the second finding can't be defended. Engagement flow: baseline screenshots + timestamped request/response dumps at session start → response shift detected → second capture set with explicit timestamp delta → both packaged together.
- **`m365-entra-attack`**; The single richest source of mid-engagement IR signal in modern engagements. Engagement flow: M365 spray triggers AADSTS50053 lockout → baseline lockout policy captured → if lockout window changes mid-test (e.g., from 60min to 24hr) → `mid-engagement-ir-detection` captures the policy change as a finding ("CA policy hardened mid-engagement; defensive response measured").
- **`enterprise-vpn-attack`** + **`vmware-vcenter-attack`**; Critical-infrastructure CVE exploitation is the highest-noise activity; expect SOC to patch within hours. Engagement flow: confirmed VPN/vCenter CVE → baseline capture BEFORE exploitation attempt → if appliance updates mid-test, capture as defensive-action finding → report both the original CVE AND the IR-response.
- **`redteam-report-template`**; IR-observation findings get their own Subject in the deliverable, framed differently from technical-vuln findings. Engagement flow: `mid-engagement-ir-detection` captures behavior-change event → `triage-validation` 7-Question Gate (specifically: "is the behavior-change attributable to my activity?") → `redteam-report-template` packages as a "client capability observation" with explicit timeline and detection-latency metric.
- Process guidance only; it does not authorize probing or exploitation; those steps need their own authorized skill and confirmation gate.