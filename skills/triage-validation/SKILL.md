> **Scope of "STOP" in this skill:** This skill's gates kill INDIVIDUAL FINDINGS that fail validation. They do NOT authorize stopping the engagement. Killing a finding via the 7-Question Gate just means *that finding* doesn't get submitted; every other test class in the engagement is still pending. See `redteam-mindset` "DO NOT STOP primary directive" for the coverage-axis rule.
2. Request: [exact HTTP method, URL, headers, body; copy-paste ready]
3. Changelog/CHANGELOG.md; does it mention this behavior?
4. API docs / design docs; is it documented as intended?
## THE LAYER-ORDERING TRAP: read before claiming any auth bypass
> "I sent an unauthenticated request and got back `400; field X is required`.
# malformed body: trips the sanitiser, which runs FIRST
# same endpoint, well-formed empty object: now auth is reached
the `{` character before auth ran; every one of those endpoints returned `401` to
  well-formed body still returns it, that is a real signal; proceed to Q6.
[ ] Bug is REAL; confirmed with actual HTTP requests, not code reading alone
[ ] Bug is IN SCOPE; checked program scope page explicitly
[ ] Reproducible from scratch; can reproduce starting from fresh session
[ ] Evidence ready; screenshot, response body, or video
Before labelling any finding **Critical** or **High** anywhere in your notes or report; write out the answer to each of these as a one-liner. If you can't answer concretely, **the severity is wrong**.
  ; "Primitive confirmed at layer N" ≠ exploitable. Multi-stage chains require ALL stages validated before the severity matches the chain's top end.
  ; "RCE on the SP front-end web server" is concrete. "Could lead to RCE" is not; that's High at best, often Medium.
  ; Twice = once during discovery, once for the report screenshot/PoC. Not "I'm sure it would work."
  ; If yes, the chain is not Critical. Document it as "primitive present" at lower severity until the gate is bypassed.
  ; Many programs cap "info disclosure with no concrete impact" at Low/Info regardless of the data type. Read the program scope.
**Lesson from an authorized engagement:** JWT `alg:none` was initially labelled **Critical** based on the signature-bypass primitive being confirmed at the audience-validation layer. Subsequent testing showed the issuer-trust check still rejected unsigned tokens; the full ATO chain did not complete. Finding had to be retracted. If the Pre-Severity Gate had been run on the original draft, Q1 would have killed the Critical label before submission.
"The API returns more fields than necessary" (sensitivity matters; is it actually sensitive?)
Reporting B saying "similar to A in my other report"; fresh Gate 0 for every bug
Overclaiming severity; triagers trust you less next time
Under-describing impact; triager doesn't understand why it matters
When a previously-claimed finding fails reproduction; **never silently drop it.** Document the retraction in the report's appendix. This proves to the triager that you validate your own work, and it saves them from chasing a phantom you've already disproved.
- **Why it looked like a bug:** <root cause of the false positive; e.g., natural marker collision, network jitter, status-code-only confidence>
**Concrete retractions from an authorized engagement; pattern reference:**
- **X-Forwarded-Proto reflection**; looked like header reflection across 4 pages; was the literal word `javascript` in SP help-link hrefs. Cause: non-unique marker.
- **Host header `:80@evil` bypass**; 200 OK on a path that normally 403s; body byte-identical to baseline (8341 bytes). Cause: status-code-only confidence; ELB Host normalisation dropped the `@evil` portion.
- **`download.aspx` file-existence oracle**; looked like a file-existence differentiator across path types; was SP's file-extension blocklist (`.ashx`/`.asmx`/`.svc`/`.config` always blocked regardless of existence). Cause: confusing server-side policy with file state.
- **`Administrator` timing leak**; single-shot 1527 ms vs ~700 ms control on Authentication.asmx Login; n=80 interleaved reproduction collapsed every group to 685-716 ms. Cause: single-sample statistical claim.
- **`report-writing`**; When all 7 questions pass and the Pre-Severity Gate is clean. Workflow primitive: this skill is the gate that runs BEFORE `report-writing`; only findings that clear all 7Q + 4 pre-submission gates get the report-template handoff.
- **`bugcrowd-reporting`**; When a Bugcrowd VRT mapping is needed for an accepted finding. Workflow primitive: after this skill validates the finding, `bugcrowd-reporting` decides the VRT category and severity-request paragraph.
- **`evidence-hygiene`**; When the validated finding needs PoC evidence captured. Workflow primitive: this skill says "Q6 requires proof of impact"; `evidence-hygiene` provides the capture-and-redact protocol for that proof.
- **`security-arsenal`**; When checking the always-rejected / conditionally-valid tables. Workflow primitive: this skill's "Never Submit List" and `security-arsenal`'s "Always Rejected" table are aligned; either entry-point lookup decides whether a primitive is reportable alone or only with a chain.
- **`bb-methodology`**; When Phase 5 (Validate & Report) starts. Workflow primitive: Phase 5's pre-report gate explicitly invokes `/validate` (this skill's 7Q gate) before any report is drafted.
1. **URL echo dressed as reflection**; payload appears in the response body because the response IS the URL. Q1 (is this a real HTTP request that does something on the server) kills it.
2. **Word collision dressed as marker hit**; the canary string `XSS-test` matched a CSS class name, not your injection. Marker Discipline + Q1 kill it.
3. **Server policy mistaken for state oracle**; `download.aspx?file=foo.config` always returns "blocked" regardless of whether the file exists. Q6 (impact beyond technically possible) kills it: there's no oracle, just a deny-list.
4. **200 OK without leak**; status code differs from baseline 403; body is byte-identical. Body-Diff Rule + Q6 kill it.
A authorized SharePoint engagement nearly submitted a Critical when the chain didn't actually complete end-to-end; a primitive that read auth state was conflated with a primitive that mutated it. The Pre-Severity Gate (run all 7 questions specifically against the **Critical claim**, not the generic "is this a bug" claim) would have caught it.
If a finding stops reproducing 24h after submission; retract preemptively. Two reasons:
The retraction template in the RETRACTION DISCIPLINE section above is the canonical format. Don't silently delete; append a retraction appendix to the engagement report instead.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.