Produce code that is correct, clear, appropriately robust, idiomatic and concise; in that priority order.
1. **Code-token inefficiency**; the artifact itself is bloated (unnecessary lines, boilerplate, over-abstraction)
2. **Generation-token inefficiency**; how the agent operates during a session (full-file rewrites, unrequested files, prose padding before/after changes)
## Workflow: apply to every coding task
### Step 4: Guardrail check: run this before presenting
### Step 5: Present output: generation-token rules
- Add prose blocks before/after code explaining what you're about to do or just did; just do it
- Restate what changed in a paragraph after showing the diff; the diff is self-evident
- No `// TODO: add error handling`; either add it or don't
- If you believe a test file would be valuable, offer it in one sentence after the main output; don't generate it unasked