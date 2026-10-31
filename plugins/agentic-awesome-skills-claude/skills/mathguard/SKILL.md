description: "Math-heavy escalation for n >= 10^6; Bloom, HyperLogLog, Count-Min, MinHash/LSH, FFT, JL projection, sweep line. Use when classical O(n log n) is the floor and approximate or math wins."
# mathguard: Math-Heavy Optimization for AI Code
`lemmaly` makes you pick the right classical algorithm. `mathguard` kicks in when the classical algorithm is already optimal but **mathematics gives a better bound**; usually by accepting bounded approximation, exploiting structure, or moving to a smarter algebraic space.
   - If the caller needs exact and there is no exact win, say so and stop; do not silently degrade to approximate.
   - The problem is I/O-bound; the math win disappears behind network/disk.
Before suggesting a math-level technique, your message must contain; in this order:
1. **The classical floor**; what is the best non-mathy algorithm and its Big-O? ("Hash join is O(n+m); we're already there.")
2. **Why classical is not enough**; n too large, space blows up, real-time deadline, etc.
3. **The math technique**; named (rule 2).
4. **Exact or approximate**; with ε if approximate (rule 1).
5. **The new bound**; with one-line derivation (rule 4).
6. **The trade**; buys/costs (rule 3).
7. **When NOT to use this**; at least one disqualifier.
## Playbook: math technique → problem → win → caveat
| Multiply two polynomials / big integers | O(n²) | **FFT / NTT / Karatsuba** | FFT/NTT O(n log n); Karatsuba O(n^log₂3) | Floating FFT loses precision; use NTT for integers |
## Canonical example: counting distinct users
### Without the protocol: silent OOM, or worse, silent billing error
// "Just use a Set"; silently OOMs the box at ~50M strings
Or worse; proposed *with* a HyperLogLog "for performance" but plugged into the billing pipeline, which keys off the result. Billing then sees 49.7M instead of 50.0M users and a fraction never get charged.
### With the protocol: auditable HLL
// Caller acceptance: confirmed; dashboard product owner accepts ±2%, written in PR.
The first version is not "no HLL"; it is "HLL without writing down ε and who tolerates it." The second is identical in technique but auditable: ε is in the comment, the caller is named, the disqualifier (billing) is explicit.
| "A `set` works; I'll flag the memory issue in a comment." | Noticing the problem is not solving it. If memory is the budget, ship the structure that respects it. |
| "Approximate is risky; I'll do exact and let it OOM later." | Silent OOM at 3am is riskier than a stated 0.81% error. State the ε, pick parameters, ship. |
| "JL projection feels too lossy for embeddings." | At ε = 0.1, JL preserves pairwise distances within 10%. This does not guarantee preserved neighbor rankings or ANN recall; measure recall, do not eyeball. |
## Red flags: STOP
- **Not for exact-required pipelines.** Any system where the result is a primary key, dedup key, billing input, or auth decision is out of scope; keep classical.
- **Library quality varies.** Bloom / HLL / MinHash implementations differ in seed strategy, hash function, and memory layout; pick a maintained library and pin the version.
- **Team-familiarity risk.** A technique nobody can debug at 3 a.m. is a liability; write the maintainer note next to the trade-off.
- `lemmaly`; gateway; pick the classical algorithm first before reaching for math.
- `invariant-guard`; for stating ε-bounds as part of the postcondition of an approximate algorithm.
- `complexity-cuts`; when baseline code already exists and the bottleneck is CPU/memory, not approximation.