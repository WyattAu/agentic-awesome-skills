# Comments and Formatting: Clean Code Chapters 4 and 5
The foundational rule: **"Don't comment bad code; rewrite it."** Comments are failures to express intent in code. Every comment is a candidate for rename or extract.
- **Legal headers**; license boilerplate, copyright.
- **Intent**; explaining *why* a decision was made when the choice is non-obvious. Example: `// Use exponential backoff to avoid hammering the rate limiter during retries.`
- **Warnings of consequences**; `// This function is called during transaction commit; do not raise.`
- **TODOs**; sparingly, with a tracking ticket reference. `// TODO(JIRA-1234): switch to streaming once API supports it.`
- **Public API documentation**; docstrings that document *contract* (preconditions, postconditions, raises), not body.
- **Amplification**; calling attention to something non-obvious. `# The `+ 1` accounts for the inclusive end of the range; see RFC §3.2.`
- **Commented-out code.** Delete it. If you need it back, git has it. Commented blocks are toxic; readers don't know whether to trust them.