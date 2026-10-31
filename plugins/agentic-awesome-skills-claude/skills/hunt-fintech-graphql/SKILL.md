Generic GraphQL bugs (IDOR, mass assignment, introspection, batching abuse; see `hunt-graphql`)
  in its own right; this barely exists in non-financial GraphQL APIs.
  `linkedBankAccount` alongside `displayName` and `email`; one missing field-level authorization
- Apollo Federation with a dedicated `ledger` or `payments` subgraph; check for the subgraph's own introspection being reachable directly, bypassing the gateway's stitched-down schema
- Custom `Money`/`Decimal`/`BigDecimal` GraphQL scalar in the schema (`scalar Money`); the parser for this scalar is worth fuzzing directly
   `transfer*`/`withdraw*`; also `redeemRewards`, `applyCoupon`, `upgradeTier`,
   writes are the ones worth racing; see Stage 4.
   means idempotency isn't enforced server-side; replay = double-execute.
4. **Test decimal/precision edge cases** on every amount-accepting argument; see Payload section.
   **source account belongs to the authenticated caller**; not just that *some* account with
  ; but sometimes only one direction does).
   type from every context that returns it; not just the profile screen. A `transaction` type
   check; e.g. an input object with a client-settable `status` or `override` field that a normal
   the same rate; a TOCTOU window here is a direct arbitrage bug.
9. **Combine alias batching with money-movement mutations** to test for double-spend; see
Top-level mutation fields execute serially under the GraphQL specification. If more than one alias reports success, inspect committed sandbox ledger effects: this can show missing single-use validation/idempotency, but does not by itself prove concurrency or a race; see `hunt-race-condition`
   separate sequential statements instead of inside a single transaction/lock; the race window
4. **Idempotency keys are stored but never checked before executing the write**; the key is
   present**; see methodology step 5. Debiting looks dangerous so it gets reviewed; the "does
   optional fields; nothing at the resolver layer strips those fields for non-admin callers.
Money-movement findings need a stricter bar than a typical GraphQL IDOR; "the query returns
   and after; a state change (not just a `200`/success response body) is the proof.
3. **Does the finding move value the attacker didn't have, or reveal data they shouldn't see** -
   `hunt-source-leak`-class finding, not a fintech-logic one; don't conflate the two in a report.
- **`hunt-graphql`**; parent skill for generic GraphQL discovery, introspection bypass, node-ID
- **`hunt-business-logic`**; coupon/reward double-redemption and other logic-flaw patterns
- **`hunt-race-condition`**; the escalation path once alias batching alone confirms a
- **`hunt-api-misconfig`**; mass assignment and JWT-claim tampering patterns apply directly to
- **`hunt-idor`**; the source-account-vs-destination-account asymmetric authz pattern (step 5) is
- **`evidence-hygiene`**; balance screenshots and ledger-entry PoCs need the same cookie/PII
- **`triage-validation`**; apply Gate 0 above before drafting; a fintech program's triage team
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.