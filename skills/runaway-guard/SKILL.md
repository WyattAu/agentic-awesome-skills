# runaway-guard: $-Cost is the Third Complexity Dimension
Every loop has time complexity and space complexity. A loop that calls a paid API has a third: **dollars per execution**. The model tracks the first two automatically. It does not track the third, so it ships code where a single bug; a retry without bound, a stream reconnect storm, an agent that re-queues itself, a webhook that fires the same job twice; silently spends real money.
**Violating the letter of these rules is violating the spirit of the skill.** "I'm only testing locally" is the exact rationalization that ships the $200 bill; local code hits the same paid API as production.
A cap only in code can be bypassed by a bug in that code. A cap only at the provider can be hit during normal usage and degrade the product. You need both. If you cannot state both in one sentence each, you have not designed the call site; you have written a wish.
   - **Max $ per run:** `max_calls × unit_cost`; compute it, don't estimate.
   // ✅ Concrete bound; wallet invariant explicit.
   - Cap counts across the whole pipeline, not just one library; Inngest retries × SDK retries × your own retry wrapper multiply.
   - In-process: `p-limit`, semaphore, or batched `Promise.all` chunks; never an unbounded `Promise.all(items.map(...))` on a paid API.
Before producing code that calls a paid API, your message must contain; in this order:
8. **The code**; with the cost contract in a comment above the call site.
## Worked trap: the Inngest + Fal $200 night
| 7. Amplifier audit | "Self-rescheduling jobs"; Inngest's retry IS self-rescheduling. The audit forces you to consider it. |
Note: the bug in `fetchPrompts` is still there. The protocol does not fix that bug; it bounds a single batch to 50 items, but retries, duplicate events and ambiguous failures can still recharge work. No $2.50 run cap is established without a shared spend reservation and verified provider deduplication. That is the entire point of defense in depth.
| Empty input list | 0 calls, 0 cost, return early; do not even auth |
- Use a named constant (`MAX_IMAGES_PER_RUN`, `MAX_AGENT_STEPS`) for the bound; never a magic number inline.
- Wrap fan-out in `p-limit` or equivalent; never raw `Promise.all` over a paid API.
- **invariant-guard**; when the loop or recursion calling the paid API has no written termination measure. Termination is a precondition for a wallet cap; invariant-guard establishes it, runaway-guard bounds the cost.
- **complexity-cuts**; when the runaway has already shipped and you are diagnosing an unexpected bill. Treat the unintended fan-out as a complexity bug, write a characterization test, then transform one step at a time.
- **lemmaly**; when designing a new agent loop or batch pipeline that fans out paid calls. Pick the algorithm and data structure first; come back here for the wallet invariant per step.
- **mathguard**; when the cost driver is compute on a sublinear-algorithm problem (vector search, sketching, FFT) rather than per-call billing; mathguard sets the algorithmic floor that determines per-call work.
| "The API call is cheap, $0.001." | At 10,000 unintended invocations that is $10; and the count is exactly what you failed to bound. |
## Red flags: STOP and write the cost contract first
- "Just for tonight"; overnight is exactly when runaway loops bill the most.
- **Not a billing system.** runaway-guard enforces *intent* (caps, contracts, audits) at code-write time. It does not meter spend in production; pair it with the provider's hard cap and observability (LLM-cost dashboards, log alerts) for runtime enforcement.
- **Streaming and per-token pricing.** For per-token APIs (Anthropic, OpenAI), `max calls` is a proxy; the real cap is `(max input tokens × input-token rate) + (max output tokens × output-token rate), summed across all attempts and including billable tools, cache tiers and other charges`. Adapt the protocol: replace `max calls per run` with `max tokens per run`.
- **Does not replace incident response.** When a bill has already arrived, escalate to `complexity-cuts` for the corrective rewrite; runaway-guard prevents the next one, not the current one.