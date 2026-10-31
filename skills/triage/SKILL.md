description: Move issues and external PRs through a state machine of triage roles; categorise, verify, grill if needed, and write agent-ready briefs.
Use when this workflow matches the user request: Move issues and external PRs through a state machine of triage roles; categorise, verify, grill if needed, and write agent-ready briefs.
If this repo treats external pull requests as a request surface (see the issue-tracker config), triage covers them too: **a PR is an issue with attached code**; same roles, same states, same machine, with a few deltas marked "for a PR" below. Resolve a bare `#42` to an issue or PR per the tracker config.
- [AGENT-BRIEF.md](AGENT-BRIEF.md); how to write durable agent briefs
- [OUT-OF-SCOPE.md](OUT-OF-SCOPE.md); how the `.out-of-scope/` knowledge base works
- `bug`; something is broken
- `enhancement`; new feature or improvement
- `needs-triage`; maintainer needs to evaluate
- `needs-info`; waiting on reporter for more information
- `ready-for-agent`; fully specified, ready for an AFK agent
- `ready-for-human`; needs human implementation
- `wontfix`; will not be actioned
These are canonical role names; the actual label strings used in the issue tracker may differ. The mapping should have been provided to you - check whether `/setup-matt-pocock-skills` is installed; it is not bundled here. If absent, collect the mapping without installing or changing configuration automatically.
State transitions: an unlabeled issue normally goes to `needs-triage` first; from there it moves to `needs-info`, `ready-for-agent`, `ready-for-human`, or `wontfix`. `needs-info` returns to `needs-triage` once the reporter replies. The maintainer can override at any time; flag transitions that look unusual and ask before proceeding.
1. **Unlabeled**; never triaged.
2. **`needs-triage`**; evaluation in progress.
3. **`needs-info` with reporter activity since the last triage notes**; needs re-evaluation.
When PRs are in scope, include external PRs in these buckets and tag each line `[PR]` or `[issue]`. Discovery surfaces only *external* PRs (the tracker config defines who counts as external); a collaborator's in-flight PR is not triage work. This filter is discovery-only; an explicitly named PR is always triaged regardless of author.
1. **Gather context.** Read the full issue or PR (body, comments, labels, author, dates; for a PR, the diff too). Parse any prior triage notes so you don't re-ask resolved questions. Explore the codebase using the project's domain glossary, respecting ADRs in the area. Run two checks against the codebase: (a) **redundancy**; search for an existing implementation of the requested behavior by domain concept (not just the request's wording), and report where you looked. If apparently found, verify the reported version and behavior before recommending already-implemented closure (step 5); a similarly named implementation may still have the reported bug. (b) **prior rejection**; read `.out-of-scope/*.md` and surface any that resembles this request.
2. **Recommend.** Tell the maintainer your category and state recommendation with reasoning, plus a brief codebase summary relevant to the request; including whether it's already implemented. Wait for direction.
3. **Verify the claim.** Before any grilling, check that the claim holds up. For a bug, reproduce it from the reporter's steps. For a PR, confirm the diff does what it claims; check it out, run the relevant tests or commands. Report what happened: confirmed (with code path), failed, or insufficient detail (a strong `needs-info` signal). A confirmed verification makes a much stronger agent brief.
4. **Grill (if needed).** If the request needs fleshing out, use `/grilling` and `/domain-modeling` only if installed and within the user-authorized scope; otherwise ask focused questions directly; grill it into shape one question at a time, sharpening domain terms and updating `CONTEXT.md`/ADRs inline as decisions land.
   - `ready-for-agent`; post an agent brief comment ([AGENT-BRIEF.md](AGENT-BRIEF.md)).
   - `ready-for-human`; same structure as an agent brief, but note why it can't be delegated (judgment calls, external access, design decisions, manual testing).
   - `needs-info`; post triage notes (template below).
   - `wontfix`; close, with the comment depending on *why*:
     - **Already implemented**; the change already exists in the codebase. Point to where it lives; do **not** write to `.out-of-scope/` (that KB is for *rejected* requests, not built ones).
     - **Rejected (bug)**; polite explanation, then close.
     - **Rejected (enhancement)**; write to `.out-of-scope/`, link to it from a comment, then close ([OUT-OF-SCOPE.md](OUT-OF-SCOPE.md)).
   - `needs-triage`; apply the role. Optional comment if there's partial progress.