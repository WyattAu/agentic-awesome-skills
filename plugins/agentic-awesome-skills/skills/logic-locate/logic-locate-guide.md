# Fault Locate: Step-by-Step Guide
Do not start from `main()`; start from the function closest to the failure.
Symptom: TypeError at service.py:7; 'NoneType' object is not subscriptable on `result['transaction_id']`.
Root: gateway.py returns None on amount==0; service.py assumes dict unconditionally; L6.
If yes, the hypothesis is confirmed. If not, revise; the actual fault may be earlier or in a different branch.
The fault is often not in the function being looked at; it is in a callee that doesn't behave as assumed (L6), or in a name that resolves differently (L1).
- The specific line/expression where the divergence **originates** (not the crash site; the cause of the crash)
   - **High:** trace fully confirms; Premises→Trace→Divergence form a complete chain from cause to symptom.