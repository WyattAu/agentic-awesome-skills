# infinity: Input Boundary & Validation Protocol
> Nothing untrusted ever reaches the core; it is stopped before contact. No external data touches the codebase raw. Every boundary where data enters the system must have a filter.
### PHASE 1: Boundary Detection
### PHASE 2: Classify Each Input
INFINITY; BOUNDARY MAP
### PHASE 3: Mandatory Filter Layer
- If a required field is missing, reject; do not use a fallback that hides the problem
// WRONG; using raw input directly
// RIGHT; validate before use
### PHASE 4: Self-Check Before Done
INFINITY; VERIFICATION
If any `UNTRUSTED` or `SEMI-TRUSTED` input reaches logic, storage, or rendering without a filter; the AI flags it. It does not silently pass.
- **No assuming shape.** Even if the API "always" returns a string; validate it.
- **No filtering in the wrong place.** Filters go at the entry point; not somewhere downstream after the data has already been used once.
| 1; Detect | List all entry points in scope | ❌ No |
| 2; Classify | Assign trust level to each input | ❌ No |
| 3; Filter | Write filter layer for all UNTRUSTED + SEMI-TRUSTED | ✅ Yes |
| 4; Verify | Trace each input, confirm filter exists | ❌ No |