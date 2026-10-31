diff). This skill does that review from your agent over the **UI Verify MCP**; read what changed, look
server in your agent; don't hand-roll `curl` against the endpoint.** Over raw `curl` the image tools come
**Claude Code**; run in the project root:
**Cursor**; add to `.cursor/mcp.json` (commit it to share), then log in from Cursor's MCP settings:
`render_diff_image` (inline pixels); see the tool table below.
| `get_build` | Lean triage of one build; the gate + `counts {total, changed, failed, unchanged}`, a build-wide `comments {total, unresolved}` review-comment tally, the AI tally, and the **first page (25)** of changed/failed stories (each with a `diffResultId`, diff %, and when AI review is on the judge's `aiVerdict`, `aiConfidence`, and for a regression `aiFlagReason` = its one-line "what looks unintended"), the page carrying a `nextCursor`. It does **not** dump the unchanged list; `counts.unchanged` is the signpost; page the rest with `list_build_stories`. `comments.unresolved > 0` means a human/agent left an open note; read it with `list_comments` before you accept | one of `commitSha` / `prNumber` / `buildId` |
| `list_build_stories` | Page through one build's stories by status; the only way to browse the full changed / failed / **unchanged** set beyond `get_build`'s first page. `counts.unchanged` from `get_build` is the exact count it pages. Every changed-story row (here and in `get_build`) carries its own `comments {total, unresolved}` | selector, `status: changed \| unchanged \| failed`, `cursor?`, `limit?` (default 25, max 100) |
| `get_diff` | Per-story diff metrics + **presigned image URLs** (baseline / candidate / diff) you can download to a file or link straight into a PR; when AI review is on, the judge's full call: `aiVerdict`, `aiConfidence`, `aiSummary` (what changed), `aiReasoning` (why), `aiFlagReason`; and a per-story `comments {total, unresolved}` (same signpost as `get_build`, scoped to the one story). Pass `storyId` to pull one story **even if it didn't change**; you get its current baseline URL, so you can show "identical to baseline" on a passed build | same selector, optional `storyId` |
| `render_diff_image` | The actual **pixels** of one image, inline for the vision model to look at (not a URL): `baseline` / `candidate` / `diff`, or `before_after` for a before-and-after crop zoomed to the changed region; one crop per region, stacked, when the story moved in several far-apart places (a header and a footer). When the story has an unresolved review comment, a text note saying so comes **before** the image | `diffResultId` (or `storyId` for a story that didn't change), `which` |
| `get_pr_changeset` | The **cumulative** "This PR vs base" changeset: what the *whole pull request* did to the UI vs the branch it merges into; `new` / `changed` / `removed` stories, each with review status (and the judge's verdict when AI review is on), plus `counts {new, changed, removed, unchanged}` and the **first page (25)** of each list. **Distinct from `get_build`**, which is one commit's gate: this survives in-PR accepts **and** merge, and catches an added-then-accepted story and a **removed** one that no single build's diff can see | one of `commitSha` / `prNumber` / `buildId` |
| `accept_build` | Accept **every** changed story in a build at once. **Refused, accepting nothing,** while any changed story has an unresolved review comment; the error names those stories | same selector |
| `list_comments` | Read the review comments a designer/QA (or another agent) left on a build's diffs; every live comment across the build, each carrying its `diffResultId`, `authorType`, `body`, `side`, `parentId` (a reply's thread root), `anchor` (a point/region on the image), and `resolved`. This is the thing `get_build`/`get_diff`'s `comments` count points you to | one of `commitSha` / `prNumber` / `buildId` |
| `post_comment` | Leave a comment (or a threaded reply) on one story's diff; e.g. reply to a reviewer's question, or flag a regression back to them. Authored as the **agent**; a new root comment is refused if a newer build exists on the branch (comment on the latest), but replies are always allowed | selector + `diffResultId`, `body`, `side?`, `anchor?`, `parentId?` (reply to a root's id) |
| `update_comment` / `delete_comment` | Edit or remove a comment; the agent may only touch **agent-authored** comments | `commentId` (+ `body` for update) |
## Recipe 1: "Triage this build"
Bucket the changes so the human sees signal, not 60 rows. Call `get_build` for the PR; it carries the
  *whether the result is good*; a code block that went dark-on-dark, text that lost contrast, a
  `get_diff` story shows `comments`), a designer/QA or another agent has left a note on this build -
1. **Regression; a real change that's wrong**; content changed and the after is broken or worse:
   itself. **A regression the current PR *caused* is still a regression**; "my diff explains it"
2. **Intended change**; content changed and the after is correct and matches the PR's stated intent
3. **Cosmetic reflow**; real diff pixels, but the diff image shows the *same* content shifted a few px
4. **Candidate rendering noise**; localized anti-aliasing or nondeterministic pixels verified from both images. No percentage threshold proves noise; even one pixel can alter a critical indicator.
refutes its `aiFlagReason`; not "it's a colour change, probably fine." When evidence is inconclusive, leave the story unresolved and explain the uncertainty.
> **⚠️ Regressions (2)**; real changes that look wrong, do NOT accept: `SetupWizard/Playwright` (8.3%,
> code block now dark-on-dark / illegible; judge flagged `regression`), `Card/Compact` (5.1%, title
> **1. Intended changes (11)**; content changed, looks correct, accept candidates: `Hero/Default` (14%,
> **2. Cosmetic reflow (9)**; real pixels, vertical shift only: `EventStatusCard/All Variants` (4.1%,
> **3. Candidate chart anti-aliasing noise (6)**; inspect the affected pixels before acceptance: `FunnelChart/Smaller
> **New stories (12)**; first baseline, nothing to compare: `Button/AllVariants`, …
Never accept from inside the triage step; surface the buckets and let the human decide, or do it
## Recipe 2: "Summarize the visual regressions"
Same read, but output the changes that need a human; the ⚠️ regressions first, then the intended
content changes; in a PR-comment shape, one line each, no reflow/noise. This is the comment to drop on
> 🔎 **Visual review; 1 regression, 2 intended** (of 67 flagged; 64 reflow/noise, listed below the fold)
> - ⚠️ `SetupWizard/Playwright`; code block dark-on-dark, illegible (8.3%, judge: regression)
> - `Pricing/Card`; CTA button color changed blue→green (12.4%)
> - `Nav/Header`; logo 8px larger (3.1%)
## Recipe 3: "Accept all the new baselines"
Don't let "the headline change is intended" halo onto the whole build; a global CSS or theme change
`accept` / `ignore` / `deny` instead; accept the intended ones, ignore the noise, leave real
## Recipe 4: "Put the before/after in the PR"
  several far-apart places); the least to eyeball. Save it and attach it.
## Recipe 5: "What did this whole PR do to the UI?"
`get_build` answers one commit; *is anything unreviewed on this build*. It does **not** prove the
**removed** story (a deleted or renamed baseline; no build renders it, so no per-build diff flags it).
  don't "re-run and see." Say so and flag it for a human; the likely cause is a baseline that moved when
The changeset is GA; no flag to enable, and on a **public** project anyone can read it for a build
## Recipe 6: "Answer or resolve a review comment"
- **Reply in the thread**; `post_comment { diffResultId, body, parentId }` where `parentId` is the root
- **Flag a regression back**; `post_comment { diffResultId, body, anchor? }` as a new root, to point the
- **Resolve**; `resolve_comment { commentId, resolved: true }` once the note is addressed (you made the
Your key authors comments as the **agent**, and can only edit/delete comments it authored; never touch a
A build with no changes shows no triptych; but the components still rendered, and "nothing changed" is
  `which: "baseline"` AND `which: "candidate"` and look at both; the diff mask shows where, the pair
  changed story; only reach for it when you've confirmed there's no real regression hiding in the list.
  a note you haven't read; `list_comments` it and honor it before `review_diff` / `accept_build`. A