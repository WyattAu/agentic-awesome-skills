Feed agents the right information at the right time. Context is the single biggest lever for agent output quality; too little and the agent hallucinates, too much and it loses focus. Context engineering is the practice of deliberately curating what the agent sees, when it sees it, and how it's structured.
- **Compact deliberately**; if the tool supports it, compact/summarize before critical work
A) Follow the spec; add REST endpoint, potentially deprecate GraphQL later
B) Follow existing patterns; use GraphQL, update the spec
C) Ask; this seems like an intentional decision I shouldn't override
3. Don't invent requirements; that's the human's job
1. Add Zod schema for task creation; validates title (required) and description (optional)
| Implicit knowledge | Agent doesn't know project-specific rules | Write it down in rules files; if it's not written, it doesn't exist |
| "The agent should figure out the conventions" | It can't read your mind. Write a rules file; 10 minutes that saves hours. |