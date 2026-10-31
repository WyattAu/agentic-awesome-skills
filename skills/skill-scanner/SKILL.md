Returns JSON with findings, URL host/line locators, structure info, and severity counts. The script catches patterns mechanically; your job is to evaluate intent and filter false positives.
- **Tool assessment**: Review `allowed-tools`; is Bash justified? Are tools unrestricted (`*`)?
3. Skills about security, testing, or education commonly reference injection patterns; this is expected
This phase is agent-only; no pattern matching. Read the full SKILL.md instructions and evaluate:
5. Check PEP 723 `dependencies`; are they legitimate, well-known packages?
- **Untrusted domains**: Unknown domains, personal sites, URL shorteners; flag for review
- `Read Grep Glob`; Low risk, read-only analysis skill
- `Read Grep Glob Bash`; Medium risk, needs Bash justification (e.g., running bundled scripts)
- `Read Grep Glob Bash Write Edit WebFetch Task`; High risk, near-full access