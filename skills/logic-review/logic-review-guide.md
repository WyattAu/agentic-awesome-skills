# Logic Review: Step-by-Step Guide
Run through every applicable item in **`../_shared/semiformal-checklist.md`**; Name Resolution, Type Contracts, State Preconditions, Control Flow Assumptions. Read its "What is NOT a Premise" section before writing.
- **Class A (Self-evident):** triggering condition is visible in local code; e.g., dereference after explicit nil check, index beyond bounds, unchecked type assertion, resource with no release on a visible exit path.
- **Class B (Invariant-dependent):** triggering condition requires an implicit external assumption to be false; e.g., "all groups always contain at least one alias" or "callers never pass nil here." Requires a reachability probe in Step 5 before reporting.
- **Control/resource paths (L5/L8):** every early return, throw/raise, catch/except, break/continue after acquisition or required post-condition. **L5 exhaustive enumeration:** when a required side-effect (audit log, metric, notification, state update) must run on every exit, verify EVERY exit path independently; `return`, explicit `raise`/`throw`, implicit raises from callees, `break`, `continue`. Name each skipped path separately in the finding; a finding that identifies only the first skipped path is incomplete. Example: "Two paths skip `audit_log.record`: (1) `return _unauthorized()` at line 3, (2) `raise BadRequest` at line 5." Note: if the skipped operation is a resource release or rollback, reclassify as L8 and apply the same exhaustive path logic there.
- **State/concurrency paths (L4/L7):** mutation during iteration, shared mutable defaults, aliases, closures, await/callback/task boundaries. For L4 also check: does the function mutate its argument AND return it (aliased-return dual contract)? For L7: does the hazard require two concurrent execution contexts; if yes it is L7, not L4 (e.g. lock-order inversion between goroutines is L7 even though it involves mutation).
  - **L7 async interleaving (critical for asyncio/coroutines):** When shared mutable state is accessed across `await`/`yield` boundaries, the bug is L7 even if no threads are involved; asyncio coroutines interleave on a single thread via the event loop. Key indicator: a check-then-act pattern with an `await` between the check and the act. Do NOT classify asyncio interleaving as L4.
1. [service.py:6] `result = charge(order)`; `charge` resolves to `payments.gateway.charge` (imported at line 1).
  5. quicksort returns arr; same object
  6. caller: original is now [1, 2, 3]; mutated without caller's knowledge
  7. caller: result is original; they are the same object (result is original == True)
  T2-step2: thread B returns instance; but object.data is still null
**Reachability gate; apply before writing any finding:**
  1. Search for invariant enforcement; constructor, validator, schema definition, or call sites visible in current scope.
  2. Enforcement found and airtight (no code path bypasses it) → **drop the candidate; do not write a finding.** Optionally record in the report Summary: "Invariant enforced at [location]; no current bug; revisit if callers or schema change."
**No-bug discipline:** When the user's question is framed as "does X cause bug Y?" and the trace conclusively shows Y does NOT apply (e.g., `defer` in Go runs on all exit paths so there is no lock leak), write a finding concluding **NO logic error** for Y with Premises → Trace → Divergence showing why. Do not fabricate unrelated findings to appear thorough; that undermines precision and triggers false positives. However, if the Risk Path Ledger from Step 3 surfaced a separate, concrete, reachable bug with a complete trace, report it as an independent finding; a no-bug verdict on Y does not suppress genuine findings on other risks. If the ledger produced no other findings, stop.
**Correctness parity principle:** Correctly concluding "no bug" is equally valuable as correctly finding a bug. A Logic Score 100 report with "no confirmed logic errors" is a professional, high-value output; not a failure. Do not lower evidence standards to produce findings. If every path in the Risk Path Ledger terminates at a confirmed safe post-condition, output Score 100 immediately. Record each path's safe-termination reason (≥1 sentence) but do NOT wrap safe paths as Suggestion findings.
**Three Rebuttal Questions** (answer each explicitly; do not skip):
- Any question finds **conclusive evidence** (a defense locatable in code) → **withdraw the finding**. Record in Summary: "Candidate [L-code] at [location] withdrawn; defense confirmed at [defense location]."
**Async/concurrency protection clause:** For L7 findings involving async interleaving (asyncio, goroutines, coroutines) or memory model hazards (volatile, happens-before), apply stricter rebuttal thresholds; withdraw when a concrete defense excludes the harmful interleaving: synchronization, immutable state, exclusive ownership, or no suspension between check and act. A primitive’s mere presence is not proof; trace its scope and memory-model guarantees. Do NOT treat the GIL, event loop single-threading, or "unlikely timing" as a defense. The GIL does not prevent asyncio interleaving; single-threaded event loops DO interleave at await points.
- `Rebuttal check: PASSED; no defense mechanism found in scope.`
- `Rebuttal check: DOWNGRADED; partial defense found at [location].`
- `Rebuttal check: WITHDRAWN; confirmed defense at [location].`
(Chinese: `反驳检查：已通过--范围内未发现防御机制。` / `反驳检查：已降级--在 [位置] 发现部分防御。` / `反驳检查：已撤回--在 [位置] 确认防御。`)
  Actual:   returns [User(active=False), User(active=True)]  (2 elements; second inactive user skipped)
Dry-run: ✅ Trigger input re-traced through fix; divergence eliminated, no regression, happy path preserved.
(Chinese: `预演：✅ 触发输入已在修复代码中重新追踪--偏差已消除，无回归，正常路径行为保持不变。`)
Dry-run: ⚠️ Initial remedy [description] failed check (b); [new issue]. Revised remedy applied.
## Step 8: Execution Verification Gate (Optional: when runtime is available)
When the environment supports code execution (CLI with shell access, sandbox, or REPL), apply this step to each **Critical** and **Warning** finding. Skip this step entirely if no runtime is available; mark findings as `unverified; no runtime available` and proceed.
- The script must contain only pure computation; NO network requests, file writes, database operations, or other side effects.
- **Script errors unexpectedly (syntax error, import failure):** Script needs fixing. Retry once with corrected script. If still broken, mark `unverified; reproducer generation failed`.
- **Script still FAILS:** Remedy is ineffective. Revise and retry (up to 3 attempts). After 3 failures, mark `unverified; remedy needs manual review`.
- **Script throws NEW exception:** Remedy introduces regression. Revert, mark `⚠️ Remedy caused regression; manual fix recommended`.
Verification: ✅ Execution-verified; reproducer FAIL → applied fix → PASS
Verification: ⚠️ Unverified; [reason]
Verification: ❌ False positive withdrawn; reproducer PASS on original code
(Chinese: `验证：✅ 已执行验证--复现脚本 FAIL → 应用修复 → PASS` / `验证：⚠️ 未验证--[原因]` / `验证：❌ 假阳性已撤回--复现脚本在原始代码上 PASS`)
# 最小可复现脚本: L4: 迭代中修改列表