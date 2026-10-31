# brooks-lint: Maintenance Harness (Orchestrator)
Use this skill when you need maintenance orchestrator for the brooks-lint plugin itself. Runs a sequential subagent pipeline; author → eval → QA → trigger-audit → release; to add or edit a skill, refresh the eval suite, keep the four manifests + README + CHANGELOG + AGENTS/GEMINI in sync, audit trigger...
## Phase 0: Context check
artifacts are the repo files themselves; agents edit `skills/`, `evals/`, manifests
## Phase 1: Classify the request
Pick the minimal set of stages. The QA stage is **never skipped**; every change is
| Add a new skill | ✓ (via `new-skill` scaffold) | ✓ | ✓ | ✓ |; |
| Edit skill / guide content | ✓ | if codes changed | ✓ | if `description` changed |; |
| Edit `_shared/` framework | ✓ | if risk defs changed | ✓ |; |; |
| Eval suite only |; | ✓ | ✓ |; |; |
| Fix trigger descriptions | ✓ |; | ✓ | ✓ |; |
| Release |; |; | ✓ |; | ✓ |
## Phase 2: Run the pipeline
1. **skill-author**; creates/edits the content. For a brand-new skill it invokes the
2. **eval-curator**; if `skill-author` reported new/changed risk codes or modes, adds
3. **consistency-qa** *(gate; never skipped)*; runs `npm run validate` + `npm test` +
4. **trigger-boundary-auditor**; run **only if a `description` field changed**. It
5. **release-manager**; run **only if a release was requested**, and **only after QA
## Phase 3: Report & feedback
- **No slash commands are created**; short forms are auto-installed by the
  maintainer authorization; release-manager stops and asks.
**Normal flow; "add a brooks-security skill":** Phase 1 selects author+eval+QA+audit.
**Error flow; QA FAIL on book-count drift:** maintainer adds a thirteenth book but