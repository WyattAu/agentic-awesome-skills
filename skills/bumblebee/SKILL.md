5. Summarize findings; especially exposure-catalog matches; in the chat reply.
## Step 1: Clarify the scan request
If the user has an advisory or exposure-catalog file ready, also ask whether they want to pass it via `--exposure-catalog`. The skill does not ship its own catalogs; point them at `threat_intel/` in the Bumblebee repo if they ask where to find ready-made ones.
Skip the questions for one-liner asks like "lauf mal ne Baseline-Scan"; just run a baseline.
## Step 2: Check Go
## Step 3: Check or install Bumblebee
Then re-check `bumblebee version`. If the binary still cannot be located, the user's `GOBIN`/`PATH` is likely misconfigured; surface the resolved `go env GOPATH` and `go env GOBIN` so they can fix it. Do not fall back to running the binary by absolute path silently; explain what is happening.
## Step 4: Run the scan
- `bumblebee-<profile>-<UTC-timestamp>.ndjson`; raw records.
- `bumblebee-<profile>-<UTC-timestamp>.report.md`; Markdown report (generated in Step 5).
Always stream stderr to a sibling `.log` file; Bumblebee emits diagnostic NDJSON there that helps explain partial scans.
Each `--root` must be an existing absolute path. Reject bare `$HOME` for this profile (Bumblebee will reject it too; surface the message clearly).
Used for incident response; broad roots are allowed but should be paired with an exposure catalog and `--findings-only` whenever possible, so the output stays focused.
## Step 5: Generate the Markdown report
The helper groups records by type and ecosystem, lists every `finding` record with its catalog entry and severity, and embeds the `scan_summary` for traceability. It is dependency-free Python 3; no `pip install` needed. Run it with the output workspace as the working directory: CLI input/output paths are confined to that directory.
## Step 6: Present results
- A short summary in chat: profile, root(s), record counts, and; most importantly; any findings with their severity. If there are zero findings, say so explicitly; silence on findings is the kind of thing that gets misread.
Do not paste large chunks of NDJSON into the chat; it is noisy and not where the user will read it.
See `scripts/render_report.py` for the report layout. Bumblebee's own documentation lives at https://github.com/perplexityai/bumblebee; consult `docs/inventory-sources.md`, `docs/transport.md`, and `docs/state-model.md` when a question goes beyond what this skill covers.