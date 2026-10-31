Comments and non-semantic cleanup are the job. Logic is never the job. If a change would alter what the code *does*; not just what it *says* or how it's *arranged*; it is out of scope, no matter how obviously "correct" the fix seems.
- The user wants a bug fixed or behavior changed (that's a different job; logic edits are out of scope here)
## Phase 0: Full Read
Before editing anything, read the entire file (or the entire relevant module if the codebase is large; not just the function in question). Do not comment or clean incrementally while still reading. A comment written without full context is a guess, and guesses are how "professional" comments end up wrong.
- Any existing project comment style already in use elsewhere in the file; match it rather than importing a foreign convention
## Phase 1: Comment Audit
| **Junk / venting** | `// wtf is this`, `// idk why but it works` | Remove tone, extract any real information underneath, rewrite professionally; or delete if it truly holds zero information |
| **Dead code comments** | Blocks of commented-out code | Remove, unless the surrounding context makes clear it's intentionally preserved (e.g., a documented fallback); flag these to the user rather than silently deleting |
| **Redundant** | `i++ // increment i` | Delete; the code already says this |
## Phase 2: Non-Semantic Cleanup
Anything beyond this; reordering logic, extracting functions, changing control flow, altering algorithms; is out of scope for this skill.
## Phase 3: Comment Rewrite / Addition
- **Use the language's idiomatic doc format** for functions, classes, and public APIs (JSDoc, docstrings, `///`, etc.); match the convention already used elsewhere in the file if one exists.
## Phase 4: Verification
- Confirm the edited file's logic is behaviorally identical to the original; comments and whitespace are the only permitted diffs, plus whatever narrow Phase 2 cleanup was done.
- If a rewritten comment removes information that was present in the original (even informally stated), that's a failure; go back and preserve it.
## Phase 5: Report Back
- Any comments flagged as "informal but contained a real warning"; confirm the information was preserved
// (flagged to user: original comment was stale; function used to return nil,
- Fabricates a rationale for a comment when the actual reason isn't knowable from context; state what's certain only
- Cannot verify runtime behavior; Phase 4 is a read-through diff check, not a test run. For anything beyond trivial files, the user should still run the actual test suite after applying this skill.
- Judgment calls on ambiguous cases (e.g., "is this dead code intentional or forgotten?") default to flagging rather than guessing; this means some cleanup will need a quick human yes/no rather than happening silently.
- Not a substitute for a linter or formatter; Phase 2 cleanup is deliberately conservative and won't enforce a full style guide (e.g., max line length rules, import ordering) unless that's trivially inferable from the surrounding file.
- Large files or unfamiliar codebases increase the risk of Phase 0 missing context that would have changed a comment's wording; flag uncertainty in the Phase 5 report rather than presenting low-confidence rewrites as settled.