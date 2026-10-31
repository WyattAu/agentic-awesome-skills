# invariant-guard: Correctness-First Coding
invariant-guard fixes the behavior. State the invariants. State the base case. State the termination argument. State the edge cases. Then write the code; and verify that the code maintains what you stated.
- Any function where you catch yourself thinking "I know this algorithm"; the trap is usually in the contract, not the loop body.
If you cannot write the invariant in one sentence, you have not designed the loop. Write code anyway and you are coding by guess; and the bug will be in the case you did not enumerate.
   - The base case(s); the smallest inputs that return without recursing.
   - The measure; a non-negative integer that strictly decreases on every recursive call (e.g. `len(xs)`, `hi − lo`, `depth`, `n`).
   - The combination; how the recursive results combine into the answer.
Before producing non-trivial code that has loops, recursion, or non-trivial state, your message must contain; in this order:
1. **Function contract**; preconditions, postconditions, and what the function returns. One line each.
2. **Loop invariants**; one per loop. (Rule 1.)
3. **Termination arguments**; one per loop or recursion. (Rules 2, 3.)
4. **Base cases and measure**; for recursion. (Rule 3.)
5. **Edge case table**; bullets, one per applicable case, with expected behavior. (Rule 4.)
6. **Illegal states made unrepresentable**; name the types or asserts that enforce invariants. (Rule 5.)
8. **Self-check**; one line per loop confirming the invariant holds at top, body preserves it, and exit implies postcondition.
## Worked trap: Boyer–Moore majority vote
These two statements are not equivalent. The loop invariant guarantees "if a majority exists, it is the candidate"; not "the candidate is a majority." Once you write both down, the gap is visible: you need a second pass to verify, or the postcondition is unmet.
  // Pass 2: verify; the voting invariant is strictly weaker than the postcondition.
- **Floyd's cycle detection**; finding the meeting point tells you a cycle exists, *not* where it starts. You need a second walk.
- **Two-pointer "find any"** vs **"find leftmost"**; the loop invariant for one does not satisfy the postcondition of the other.
- **QuickSelect partition**; the loop returns a position; the postcondition is that the element at that position is the k-th smallest. Off by one in the partition invariant silently breaks it.
- **DP with reconstruction**; the table tells you the optimum value; reconstructing the optimum path needs separate invariants on the choice array.
## Canonical example: binary search for the leftmost match
### Without the protocol: returns any match
### With the protocol: contract-driven leftmost
Same loop shape. The difference is the contract was written first; and the loop body was chosen to maintain an invariant that *implies* the postcondition.
## Edge case table: defaults to consider
- Handle every edge case you listed in step 5, or explicitly delegate ("throws on empty; caller responsibility").
- The function is performance-critical and you have not picked the algorithm; go back to **`lemmaly`** first; pick the algorithm, then state its invariants here.
- The technique is mathematical (probabilistic, FFT, geometry); load **`mathguard`**; invariants for approximate algorithms include ε-bounds, not equality.
- The code is concurrent; invariants must account for interleaving; explicitly state "single-threaded only" if that is the assumption.
| "I know this algorithm; single pass, done." | Knowing the loop ≠ knowing the contract. The trap usually lives in the postcondition the loop does not enforce. |
## Red flags: STOP and write the invariant first
- About to write `if (i === n − 1)` or `if (i === n)`; boundary suspicious, restate the invariant.
- About to write `// TODO: handle empty`; handle it now or change the type so empty is impossible.
- **Documentation is the only enforcement.** If the author skips writing the invariants, this skill cannot detect that; pair with code review or a PR template that asks for the contract.
- `lemmaly`; algorithm choice must be settled before invariants; load lemmaly first if the algorithm family is unclear.
- `mathguard`; ε-bounded postconditions for approximate / randomized algorithms.
- `complexity-cuts`; if 3+ optimization transformations have failed tests, the bug is a missing contract, not a missing optimization; escalate here.