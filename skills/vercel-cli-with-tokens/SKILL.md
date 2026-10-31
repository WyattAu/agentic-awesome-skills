description: "Deploy and manage projects on Vercel using token-based authentication. Use when working with Vercel CLI using access tokens rather than interactive login; e.g. \"deploy to vercel\", \"set up vercel\", \"add environment variables to vercel\"."
- Use this skill when the task matches this description: Deploy and manage projects on Vercel using token-based authentication. Use when working with Vercel CLI using access tokens rather than interactive login; e.g. "deploy to vercel", "set up vercel", "add environment variables to vercel".
### D) No token found: ask the user
**Important:** Once `VERCEL_TOKEN` is exported as an environment variable, the Vercel CLI reads it natively; **do not pass it as a `--token` flag**. Putting secrets in command-line arguments exposes them in shell history and process listings.
# Bad: token visible in shell history and process listings
# Good: CLI reads VERCEL_TOKEN from the environment
**If you have both `VERCEL_ORG_ID` and `VERCEL_PROJECT_ID` in your environment**, export them; the CLI will use these automatically and skip any `.vercel/` directory:
Note: `VERCEL_ORG_ID` and `VERCEL_PROJECT_ID` must be set together; setting only one causes an error.
### Quick Deploy (have project ID: no linking needed)
### Full Deploy Flow (no project ID: need to link)
**A) Git Push Deploy; has git remote (preferred)**
**B) CLI Deploy; no git remote**
- `.vercel/project.json`; from `vercel link`. Contains `projectId` and `orgId`.
- `.vercel/repo.json`; from `vercel link --repo`. Contains `orgId`, `remoteName`, and a `projects` map.
**Do NOT** run `vercel project inspect` or `vercel link` in an unlinked directory to detect state; they will interactively prompt or silently link as a side-effect. `vercel ls` is safe (in an unlinked directory it defaults to showing all deployments for the scope). `vercel whoami` is safe anywhere.
# Add a domain to the project: linked or env-linked directory (1 arg)
# Add a domain: unlinked directory (requires <project> positional)
If this project is managed by Stripe Projects. **Ask the user before running any paid or destructive plan change**; upgrades bill a real card, downgrades remove seats.
- Turbo build machines (30 vCPUs, 60 GB memory) by default for new projects; significantly faster builds than Hobby.
- Missing dependencies; ensure `package.json` is complete and committed.
- Missing environment variables; add with `vercel env add`.
- Framework misconfiguration; check `vercel.json`. Vercel auto-detects frameworks (Next.js, Remix, Vite, etc.) from `package.json`; override with `vercel.json` if detection is wrong.