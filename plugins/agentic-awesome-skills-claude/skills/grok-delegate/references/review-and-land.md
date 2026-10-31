reality, never against the self-report; and read the diff as generated code, which fails in ways a
If the diff touches existing tests, review those edits *first*; before the gate re-run means anything.
`result.json` carries Grok's own claim that the gates passed. Treat that as a claim, not evidence -
not just misreport it; that is what the test check above and the sweep below exist to catch.
- **Scope creep**; did Grok change things the brief said to leave untouched? Unasked refactors,
- **Scope shortfall**; did it do the whole task, including the edge cases and cleanup, or stop at the
- **Quiet judgment calls**; sometimes Grok makes a defensible decision the brief didn't anticipate.
Generated code fails in systematic ways that gates are structurally blind to; each of these can sit in
- **Hardcoded success or fixture data** on a path the brief says does real work; a canned
- **Catch-all error handling that returns a default** instead of propagating; the suppressed failure is
- **Unverified imports and API calls**; confirm every new dependency, method, and signature exists in
- **Dead weight**; unused imports, helpers nothing calls, unreachable branches, "Step 1/Step 2"
- **A second way to do what the file already does**; a new HTTP client, error idiom, or logging style
- **New tests that assert internals**; asserting that an internal helper was called, or mocking the
- **Near-duplicate test bodies** differing by one value; fold into one data-driven test or drop the
- **Speculative surface**; optional parameters, config flags, or abstractions with no caller in this
- **Guards for impossible cases**; null/type checks for values the code's own contract already
commit; and either way is reported to the user (see "Surface, don't absorb").
If the `guard-skills` package is installed, run the relevant guard on the diff for the full treatment -
When the gates pass and the diff holds, **you commit**; the orchestrator, never Grok. This isn't a
implementer's work; the only one you can commit from, and often the only copy at all. Never run `git checkout`, `reset`, `clean`, or a branch switch in the
workspace between those two points; however messy an interrupted run looks, inspect it first:
`git diff` is blind to the index), and open any untracked files (`??` in `git status`) directly -
verdict can legitimately be to discard; work built on a premise you have since corrected, for
example; and then `git checkout`/`clean` is the right tool. The ban is on reflexive cleanup
(`<skill-dir>` is this skill's install directory; see [dispatch-and-poll.md](dispatch-and-poll.md).)
Then review again; rework gets the same gate-rerun, test check, diff-read, and sweep as the original,
- **Stop and ask** if correct completion requires going beyond the brief; don't expand the mandate on
For a multi-task run, capture these in the progress file rather than letting them scroll past; see