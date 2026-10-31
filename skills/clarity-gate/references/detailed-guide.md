The 9 Verification Points guide **semantic review**; content quality checks that require judgment (human or AI). They answer questions like "Should this claim be hedged?" and "Are these numbers consistent?"
### Tier 2: Two-Round HITL Verification: MANDATORY
  - `clarity-gate-version`; Tool version (no "v" prefix)
  - `processed-date`; YYYY-MM-DD format
  - `processed-by`; Processor name
  - `clarity-status`; CLEAR or UNCLEAR
  - `hitl-status`; PENDING, REVIEWED, or REVIEWED_WITH_EXCEPTIONS
  - `hitl-pending-count`; Integer ≥ 0
  - `points-passed`; e.g., `1-9` or `1-4,7,9`
  - `hitl-claims`; List of verified claims (may be empty `[]`)
- `rag-ingestable`; **Computed by validators**, not manually set. Shows `true` only when `CLEAR | REVIEWED` with no exclusion blocks.
- `document-sha256`; Required. 64-char lowercase hex hash for integrity verification. See spec §2 for computation rules.
- `exclusions-coverage`; Optional. Fraction of body inside exclusion blocks (0.0–1.0).
  - `[STABLE]`; Expected low drift; still verify when accuracy or current applicability matters
  - `[CHECK]`; Verify before citing
  - `[VOLATILE]`; Changes frequently; always verify
  - `[SNAPSHOT]`; Point-in-time data; include date when citing