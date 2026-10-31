        // Step 3: Victim sells; loses 99% to fees
- **`web3-audit`**; When the target is a DeFi protocol (not just a token). Workflow primitive: this skill's 8 token-specific bug classes are a subset of `web3-audit`'s scope; if the target has lending / vault / oracle logic beyond the token contract itself, also load `web3-audit` for the broader 10 DeFi bug classes.
- **`triage-validation`**; When deciding if a rug-pull finding qualifies as a bug bounty submission. Workflow primitive: many "rug vector" observations are pre-rug warnings, not exploitable bugs in a deployed protocol; run 7Q gate to distinguish "the deployer COULD rug" (informational) from "the contract IS exploitable now" (reportable).
- **`report-writing`**; When writing up a confirmed token security finding for Immunefi / private bounty. Workflow primitive: Foundry PoC template here feeds into `report-writing`'s Immunefi body template.
- **`offensive-osint`**; When the token has off-chain infrastructure (project website, Telegram, deployer doxxing). Workflow primitive: on-chain audit is this skill's domain; deployer wallet history, social presence, and project legitimacy checks route to `offensive-osint`.
- **`bb-methodology`**; When confirming engagement mode. Workflow primitive: PART 0 separates "pre-investment due diligence" (this skill's primary use) from "Immunefi bug bounty submission" (different reporting + severity standards); the answer routes which post-audit handoff is correct.
### Solana-specific signals: 2025-2026 reality
### Honeypot detection: beyond the obvious
| Buy from random wallet | Succeeds (must; or no one would touch it) |
Cross-source verification: solana-rugcheck, rugcheck.xyz, dexscreener risk signals, honeypot.is (EVM). Never trust a single source; rug-checkers can be fooled by deferred-malice patterns (clean for first 7 days, hook activates later).
### LP lock claims: verify, don't trust
- **Unlock date**; the on-chain timestamp, not the badge text
- **Lock owner**; is it the deployer's wallet? Are there multiple lockers fragmenting the LP?
- **Extension / shortening permission**; some lock contracts allow the owner to SHORTEN the lock; that's an exit hatch dressed as a lock
- **Lock contract bytecode**; verify it's the canonical locker, not a fork with backdoors
Some "locked" LPs are 1-day locks renewed weekly; visually the same as a year lock to the buyer, but the deployer can let it lapse on any Tuesday. Always read the lock contract source on the chain explorer.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.