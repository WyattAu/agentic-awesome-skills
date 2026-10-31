# Brooks-Lint: Full Sweep Guide
Step 0 (pre-flight consent); after approval the pipeline runs hands-free until Step 8.
### Step 0: Pre-flight consent gate
0b. Show this notice verbatim with the estimate filled in. Do not paraphrase -
   ⚠️  /brooks-sweep; Full Repository Sweep & Auto-Fix
   Scope:    Four analysis dimensions run in sequence; PR code decay (R1–R6),
             structural change, or no test coverage) are NOT applied; they are
   1. **Hard negation** (`no`, `n`, `abort`, `cancel`, `取消`, `不要`): abort with "Aborted before scan; no files modified."
   4. **Question**: answer it, then re-show the notice once and wait for the next reply. If the next reply is not Consent (rule 2); whether a second question, another pause, or anything else; abort with "Aborted; did not receive consent after clarification."
### Step 1: Scope enumeration and state init
   applied config values and reuse them across all iteration rounds; do not
   - **`unresolvable`** (set): findings retired after 3 failed attempts; keyed by stable `(file, risk_code, symptom_signature)`; line ranges are evidence only and can shift. Never re-queued.
### Step 2: brooks-review pass (R1–R6 code decay)
   | **Residual** | Public API break, cross-service boundary change, no test coverage to fall back on, or remedy ambiguous. NOT applied; carried to the Step 8 residual report. |
### Step 3: brooks-test pass (T1–T6 test decay)
### Step 4: brooks-debt pass (tech debt accumulation)
Re-classify R-findings through a debt lens; same symptoms at accumulation scale:
### Step 5: brooks-audit pass (architecture integrity)
defined in `../_shared/decay-risks.md` Risk 5; use that checklist. Step 5
Most architecture findings are **Residual** by definition; they require human
### Step 6: Iteration loop
     `"Unresolved; iteration cap reached"`. Otherwise return to 6a.
### Step 7: Residual aggregation
### Step 8: Sweep report
# Brooks-Lint: Full Sweep Report
Round 1: <classification; clean / critical-only / mixed>, <N> new findings