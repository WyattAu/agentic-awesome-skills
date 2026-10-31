checkable file and runs it with receipts.** Nika is NOT another coding agent -
  pipeline); capture it as a workflow instead of re-prompting
- One-off questions or single tool calls; just answer or use a tool
- Autonomous code implementation/refactoring/PR review; use the `opencode` skill
- Interactive back-and-forth tasks; workflows are non-interactive by design
- Nika installed: `brew install supernovae-st/tap/nika`; other install
Run a real workflow; local model first:
Long runs: launch in background and poll; do not block the turn:
running; never suppress them.
narrates what it will do, the waves, the cost floor, and what it touches -
  start (exit 2, zero tokens); and since 0.99 the pre-start floor prices
- A model absent from the catalog meters as $0; a paid *uncataloged* model
  run` prints last; status, cost, trace path) back to the user verbatim
Every run writes a trace under `.nika/traces/`; the run card prints the
wants those wired into their agent client, point them at the wiring guide -
https://github.com/supernovae-st/nika-agents/tree/main/integrations/mcp -
| `nika check <file> --json` | Static pre-flight; ALWAYS before run |
| `nika doctor` | Diagnose env/keys; prints exact fixes |
1. NEVER run an unchecked workflow; `nika check` first, every time.
6. If a run fails, read `nika explain <NIKA-code>` before retrying; do not
  stay quiet until completion; for anything long, prefer `background=true` +
  fast naming the flag; always pass `--from <template>` when delegating.
- Uncataloged model ids meter as $0; never rely on `--max-cost-usd` for a
- Workflow `outputs:` are not resolved on a budget stop; per-task values