A journaling skill in the tradition of the *Market Wizards* interviews: a written record of every trade's decision process, reviewed on a schedule. The user reports a trade in plain language; *"bought 500 NVDA at 135, stop at 128, betting the post-earnings dip fills"*; and the agent writes ticker, size, and price to the user's own Notion database **plus the part every spreadsheet journal loses: the thesis, the plan, and the emotion.** If no reason is stated, it asks on the spot, because entry reasons decay overnight. Reviews grade decision quality against the user's own plan; a per-plan loss scores better than a lucky win. Core contract: never fabricate when unsure; mark `To-confirm` and batch-ask.
The companion Notion template (free, linked in the source repo) ships this schema; select values are a controlled enum, copy them exactly:
- `Entry` (title); `Ticker` (text; strikes/expiries here: NVDA / NVDA 0620C150 / ESU6)
- `Entry Price` / `Exit Price` (number) · `Entry Date` / `Exit Date` (date; expand to `"date:Entry Date:start": "YYYY-MM-DD"`; a bare value fails with HTTP 400)
- `Thesis` (text); **the soul of the journal; if missing, ask on the spot**
- `Plan` (text); stop / target / contingency; ask if missing
- `Emotion` (select): Calm / FOMO / Panic / Revenge / Boredom / Overconfidence; tag only what the user admits or what is plain in their words; don't diagnose
- `Execution` (select): Per plan / Early exit / Delayed stop / Impulse / Unplanned add; filled at close
Parse → create a row with `Status=Open`. A missing thesis is the one field worth interrupting for; ask immediately. Anything else uncertain: record what you have, put the question in `Notes`, mark `To-confirm`.
- "review my trades" → query recent `Closed` + all `Open` rows. For each closed trade, three questions: **Did the thesis play out?** (wrong thesis + profit = luck; say so). **How was the execution?** (a per-plan loss is a good trade). **What share of trades were emotion-tagged?** Write conclusions into `Review`, move `Closed` → `Reviewed`, and for every open position ask: *does the entry thesis still hold today?*
Parsing rules: infer `Market` from symbol and context (ambiguous → ask); bought put = Short exposure + note it is a long put; record only given prices/sizes; **never look up market prices to fill gaps**; "today" = the user's local trading date; confirm before writing.
       ❓ And the plan; holding through earnings, or is there a stop?
       · NVDA long; thesis played out, exited per plan. Good trade.
       · TSLA short; thesis wrong, but stopped exactly where planned. Good trade, bad outcome.
       · SPY calls; profitable, but Entry was tagged FOMO and there was no plan. Lucky, not good.
       Open positions: TSM call; ask the user whether the recorded thesis still holds; no current-market conclusion was verified.
- ✅ Ask for the thesis at entry time; it decays overnight and memory flatters
- ❌ Don't compute P&L you are unsure of; options and futures have multipliers; use the user's numbers
- ❌ A review is not a P&L total; it grades thesis and execution
- Requires the user's own Notion workspace, the companion database (duplicate the template from the source repo), and the official Notion connector granted access; connector/skill setups currently live on paid Claude tiers.
  the user for the current session via the official Notion connector (MCP); no shell commands,
- On claude.ai, Notion's write tools default to *needs approval*; the first write pops an approval prompt; expected, not a hang.
- **Problem:** `create-pages` succeeds but the date column is empty (known Notion MCP issue: [notion-mcp-server#121](https://github.com/makenotion/notion-mcp-server/issues/121); expanded date fields silently dropped).