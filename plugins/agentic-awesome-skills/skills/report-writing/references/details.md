- Use "I" not "the researcher"; you found it, own it
"This vulnerability does not require any special privileges; only a free account."
"An attacker can automate this with a simple loop; all [N] records in minutes."
- Both created via normal registration; no special access
- **`triage-validation`**; When deciding whether to write a report at all. Workflow primitive: NEVER open this skill before `triage-validation`'s 7-Question Gate passes; a finding that fails the gate should be killed, not written up.
- **`bugcrowd-reporting`**; When the target is a Bugcrowd program. Workflow primitive: this skill's body template is the foundation; `bugcrowd-reporting` overlays VRT selection, severity-request paragraph, OOS-clause rebuttals on top.
- **`evidence-hygiene`**; When PoC screenshots / HARs are being attached to the report. Workflow primitive: every artifact referenced in the "Supporting Materials" / "Proof of Concept" section gets routed through `evidence-hygiene` for cookie + PII redaction before attachment.
- **`redteam-report-template`**; When the engagement is an external red team (NOT bug bounty). Workflow primitive: confirm engagement mode via `bb-methodology` PART 0; if red-team, swap this skill out for `redteam-report-template` (different audience, different structure: Subject / Observations / Description / Impact / Recommendation / PoC).
`<asset> | <bug class> | <impact>`; three components, no fluff. Triagers read titles in roughly three seconds and use them to order the queue.
An authorized bug-bounty engagement saw P4-default findings escalated to P3 via the severity-request paragraph. The escalation isn't automatic; you have to ask, with grounded reasoning, in the first body section.
- Never paste production cookies, real user emails, or real PII into the report body; redact in the PoC step.
### Templates by platform: when they differ
- **`bb-methodology`**; When Phase 5's report-writing step starts. Workflow primitive: Phase 5 calls `/report` which loads this skill for the platform-specific template (H1 / Bugcrowd / Intigriti / Immunefi).