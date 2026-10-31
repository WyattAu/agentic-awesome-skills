description: "Natural-language time tracking: parse what the user says they did into Activity/Minutes/Date rows in their own Notion database; asking instead of guessing when unsure."
Conversational time tracking. The user reports time in plain language; *"read papers 2h, gym 1h, did a leetcode"*; and the agent parses it into structured rows (activity, minutes, date, an optional compounding tag) and writes them to the user's own Notion database through the official Notion connector. The core design is an honesty contract: **anything uncertain becomes a `To-confirm` row and gets batch-asked; never fabricated.** A log that quietly invents durations is worse than no log.
The companion Notion template (free, linked in the source repo) ships this field schema; select values are a controlled enum, copy them exactly:
- `Entry` (title); the user's words or a clear title
- `Date` (date); expand to `"date:Date:start": "YYYY-MM-DD"`; a bare `Date` value fails with HTTP 400
- `Notes` (text); clue from the user's words / your question
- **One report, multiple blocks**: "two hours on X, one hour on Y" → split into rows, but confirm whether the totals are additive (a frequent ambiguity); default additive + one To-confirm row.
- **Compounding tag**: only tag the obvious; Compounding = leaves a reusable asset (learning, building, writing); Consuming = forget-on-sight entertainment (only if the user volunteers it); leave ambiguous blank.
Agent: 3 rows need confirmation; answering in one go works:
       1. Tue "LeetCode"; how many minutes?
       2. Wed "reading"; book or papers (Reading), or course exercises (Practice)?
       3. "all afternoon on the deck"; Wed or Thu?
- ❌ Don't fabricate a duration, category, or date when unsure; To-confirm + ask
- Requires the user's own Notion workspace, the companion database (duplicate the template from the source repo), and the official Notion connector granted access to that database; connector/skill setups currently live on paid Claude tiers.
- **Mutation scope**: writes and updates rows only in the single user-granted Notion database, via the official Notion connector (MCP); no shell commands, no network fetches, no credentials handled by the skill.
- On claude.ai, Notion's write tools default to *needs approval*; the first write pops an approval prompt; this is expected, not a hang.
  **Solution:** Expand to `"date:Date:start": "YYYY-MM-DD"`; a bare `Date` value fails.
- **Problem:** `create-pages` succeeds but the Date column is empty (known Notion MCP issue: [notion-mcp-server#121](https://github.com/makenotion/notion-mcp-server/issues/121); expanded date fields silently dropped).