description: Configure this repo for the engineering skills; set up its issue tracker, triage label vocabulary, and domain doc layout. Run once before first use of the other engineering skills.
Use when this workflow matches the user request: Configure this repo for the engineering skills; set up its issue tracker, triage label vocabulary, and domain doc layout. Run once before first use of the other engineering skills.
- **Issue tracker**; where issues live (GitHub by default; local markdown is also supported out of the box)
- **Triage labels**; the strings used for the five canonical triage roles
- **Domain docs**; where `CONTEXT.md` and ADRs live, and the consumer rules for reading them
- `AGENTS.md` and `CLAUDE.md` at the repo root; does either exist? Is there already an `## Agent skills` section in either?
- `docs/agents/`; does this skill's prior output already exist?
- `.scratch/`; sign that a local-markdown issue tracker convention is already in use
Summarise what's present and what's missing. Then walk the user through the three decisions **one at a time**; present a section, get the user's answer, then move to the next. Don't dump all three at once.
**Section A; Issue tracker.**
> Explainer: The "issue tracker" is where issues live for this repo. Skills like `to-issues`, `triage`, `to-prd`, and `qa` read from and write to it; they need to know whether to call `gh issue create`, write a markdown file under `.scratch/`, or follow some other workflow you describe. Pick the place you actually track work for this repo.
- **GitHub**; issues live in the repo's GitHub Issues (uses the `gh` CLI)
- **GitLab**; issues live in the repo's GitLab Issues (uses the [`glab`](https://gitlab.com/gitlab-org/cli) CLI)
- **Local markdown**; issues live as files under `.scratch/<feature>/` in this repo (good for solo projects or repos without a remote)
- **Other** (Jira, Linear, etc.); ask the user to describe the workflow in one paragraph; the skill will record it as freeform prose
If; and only if; the user picked **GitHub** or **GitLab**, ask one follow-up:
> Explainer: Open-source repos often receive feature requests as pull requests, not just issues; a PR is an issue with attached code. If you turn this on, `/triage` pulls *external* PRs into the same queue and runs them through the same labels and states as issues (collaborators' in-flight PRs are left alone). Leave it off if PRs aren't a request surface for you.
- **PRs as a request surface**; yes / no (default: no). Record the answer in `docs/agents/issue-tracker.md`. For local-markdown and other trackers, skip this question; there are no PRs.
**Section B; Triage label vocabulary.**
> Explainer: When the `triage` skill processes an incoming issue, it moves it through a state machine; needs evaluation, waiting on reporter, ready for an AFK agent to pick up, ready for a human, or won't fix. To do that, it needs to apply labels (or the equivalent in your issue tracker) that match strings *you've actually configured*. If your repo already uses different label names (e.g. `bug:triage` instead of `needs-triage`), map them here so the skill applies the right ones instead of creating duplicates.
- `needs-triage`; maintainer needs to evaluate
- `needs-info`; waiting on reporter
- `ready-for-agent`; fully specified, AFK-ready (an agent can pick it up with no human context)
- `ready-for-human`; needs human implementation
- `wontfix`; will not be actioned
**Section C; Domain docs.**
- **Single-context**; one `CONTEXT.md` + `docs/adr/` at the repo root. Most repos are this.
- **Multi-context**; `CONTEXT-MAP.md` at the root pointing to per-context `CONTEXT.md` files (typically a monorepo).
- If neither exists, ask the user which one to create; don't pick for them.
Never create `AGENTS.md` when `CLAUDE.md` already exists (or vice versa); always edit the one that's already there.
[one-line summary of layout; "single-context" or "multi-context"]. See `docs/agents/domain.md`.
- [issue-tracker-github.md](./issue-tracker-github.md); GitHub issue tracker
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md); GitLab issue tracker
- [issue-tracker-local.md](./issue-tracker-local.md); local-markdown issue tracker
- [triage-labels.md](./triage-labels.md); label mapping
- [domain.md](./domain.md); domain doc consumer rules + layout
Tell the user the setup is complete and which engineering skills will now read from these files. Mention they can edit `docs/agents/*.md` directly later; re-running this skill is only necessary if they want to switch issue trackers or restart from scratch.