# lemmaly: Algorithm-First Proof
When in doubt, **start at lemmaly**; it is the gateway and will tell you when to escalate to its three sibling skills.
Before you write a loop, a recursion, a query, or any computation over more than a handful of items, three things must appear in your message; in this order:
2. **Name the data structure with a one-phrase reason.** Every collection-shaped value gets a deliberate choice from `Array / List / Set / HashMap / TreeMap / Heap / Deque / Trie / Graph / BitSet / Counter / LinkedList`; with the reason: "Set for expected/amortized constant-time membership under the runtime’s hash model", "Heap for top-K in O(n log k)", "Counter to fold the nested loop into a single pass". Default to hashed structures (`Set`, `Map`) for lookup inside loops. Default to streaming/iterator over materialized list when n is large.
   - I/O inside a loop (database queries, HTTP calls, file reads); consider bounded batches, bulk endpoints or streaming. Use a concurrency limit and service rate/memory budget; unbounded Promise.all over n remote calls can exhaust resources. Preserve sequential dependencies and transaction semantics.
   - Recomputing the same value in a loop; hoist or memoize
   - Re-sorting / re-grouping inside a loop; sort once outside
   - Linear scan (`.find`, `.indexOf`, `.includes`, `in list`) inside a loop; precompute an index `Map`
   - Allocating fresh structures per iteration when one can be safely reused; hoist only when values do not escape or alias across outputs/async tasks
   - Materializing intermediate collections only to iterate again; fuse into one pass
Before producing non-trivial code, your message must contain; in this order:
1. **Problem shape**; one sentence. ("Given n events with a timestamp, find the longest contiguous window where total weight ≤ K.")
2. **Input dimensions**; `n = ?`, realistic magnitude, whether hot path.
3. **Target complexity**; `time = O(?)`, `space = O(?)`.
4. **Data structures**; name them with a phrase each.
5. **Algorithm family**; one phrase.
6. **Edge cases you will handle**; empty, singleton, all-equal, n=1, n=max, overflow, duplicates. List the ones that apply.
## Canonical example: protocol vs no-protocol
### Without the protocol: ships O(n·m)
### With the protocol: O(n + m)
The first version is the default an AI ships when asked "filter the active users." The second is what the protocol forces; without changing how the code reads.
- `js-await-in-for-loop`; N+1 over network
- `js-async-in-foreach`; dropped promises
- `py-mutable-default-arg`; shared default state
- `sql-update-no-where`; touches every row
- `java-arraylist-remove-in-for-i`; index shifts; ConcurrentModification
- `cs-async-void`; exceptions unobserved; crashes the process
- `go-loop-var-capture`; pre-1.22 race on the last value
- `php-query-in-loop`; N+1 against the database
- **Math-level optimization** (probabilistic data structures, FFT, dimensionality reduction, approximation algorithms, computational geometry); load **mathguard**.
- **Algorithm correctness** (loop invariants, termination, recursion base cases, edge cases that tests miss); load **invariant-guard**.
- **Existing code with bad complexity that already shipped**; load **complexity-cuts** for the corrective transformation playbook.
| "The data is small in dev; I'll worry about scale when we ship." | Production data is never the size of dev data. The seven-step protocol takes 30 seconds. |
## Red flags: STOP and restart the protocol
- [ ] Any "fast" / "efficient" / "scales" claims have either a derivation or a measurement; `<measured: TBD>` is acceptable; an unsupported claim is not.
- `mathguard`; escalation for n ≥ 10⁶ where classical O(n log n) is the floor and probabilistic / math-heavy techniques win.
- `invariant-guard`; correctness layer for algorithms whose obvious version is subtly wrong.
- `complexity-cuts`; corrective playbook for code that already shipped with bad Big-O.