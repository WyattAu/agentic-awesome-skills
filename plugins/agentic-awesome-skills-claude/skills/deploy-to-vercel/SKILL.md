If the user belongs to multiple teams, present all available team slugs as a bulleted list and ask which one to deploy to. Once the user picks a team, proceed immediately to the next step; do not ask for additional confirmation.
If the project is already linked (`.vercel/project.json` or `.vercel/repo.json` exists), the `orgId` in those files determines the team; no need to ask again. If there is only one team (or just a personal account), skip the prompt and use it directly.
- `.vercel/project.json`; created by `vercel link` (single project linking). Contains `projectId` and `orgId`.
- `.vercel/repo.json`; created by `vercel link --repo` (repo-based linking). Contains `orgId`, `remoteName`, and a `projects` array mapping directories to Vercel project IDs.
**Do NOT** use `vercel project inspect`, `vercel ls`, or `vercel link` to detect state in an unlinked directory; without a `.vercel/` config, they will interactively prompt (or with `--yes`, silently link as a side-effect). Only `vercel whoami` is safe to run anywhere.
3. **Ask which team to deploy to**; present team slugs from `vercel teams list --format json` as a bulleted list. If only one team / personal account, skip. Once selected, proceed immediately.
### No-Auth Fallback: claude.ai sandbox
**When to use:** Last resort when the CLI can't be installed or authenticated in the claude.ai sandbox. This requires no authentication; it returns a **Preview URL** (live site) and a **Claim URL** (transfer to your Vercel account).
### No-Auth Fallback: Codex sandbox
**When to use:** In the Codex sandbox where the CLI may not be authenticated. Codex runs in a sandboxed environment by default; try the CLI first, and fall back to the deploy script if auth fails.
You likely cannot run `vercel login` or `git push`. Go directly to the **no-auth fallback; claude.ai sandbox**.
Codex runs in a sandbox. Check if the CLI is available first, then fall back to the deploy script. Go to the **no-auth fallback; Codex sandbox**.
If deployment fails due to network issues (timeouts, DNS errors, connection resets) in a sandboxed environment, follow the active environment approval policy; a timeout alone does not prove sandbox blocking. Never bypass a denied network policy. Do not escalate the initial CLI availability check; only escalate the actual deploy command.
the command with escalated permissions; want me to proceed?