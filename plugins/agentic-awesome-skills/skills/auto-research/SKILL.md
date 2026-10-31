When implementing tasks, Claude Code can encounter uncertainties; design choices, algorithm details, API usage, or best practices. This skill provides an explicit-consent research path, presents findings, and waits for user approval before writing code.
**Step 1: Propose the research boundary**; State the source to use, the exact query or
**Step 2: Research**; After approval, use web search or a browser session the user has
**Step 3: Present**; Distill findings into concise options with sources, presented to the user.
**Step 4: Await Approval**; Do NOT write code until the user says "go ahead" or picks an option.
**Step 5: Implement**; Once approved, execute with confidence.
- Does not replace expert domain knowledge; always let user make the final call
- @systematic-debugging; use when debugging Playwright interactions with ChatGPT
- @condition-based-waiting; use when waiting for GPT responses in the browser