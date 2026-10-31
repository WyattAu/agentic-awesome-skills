- [Placeholders to substitute](#placeholders-to-substitute); `{OUTPUT_DIR}`, `{COMPETITOR_SLUG}`, etc.
- [Prompt](#prompt); full subagent instruction template (paste with placeholders filled in)
- [Wave management](#wave-management); launch policy: one Agent message per run, all competitors in parallel
Main agent substitutes placeholders per competitor. Launch AFTER Step 5c fact-check completes; this lane depends on `matrix.json` cells having `sources` URLs.
TOOL RULES; CRITICAL, FOLLOW EXACTLY:
   browse cloud fetch; ALL BANNED. This is a SYNTHESIS lane, not a research lane.
4. NEVER use ~ or $HOME; full literal paths only.
INPUTS (all already exist on disk; read in one Bash call):
- {OUTPUT_DIR}/matrix.json                 # fact-checked matrix; cells
TASK; produce three sections, every claim traceable to an input bullet
1. LANDMINES (3-5 items); concrete verifiable facts that HURT
   - States a specific, verifiable fact (not "their results are weak" -
2. OBJECTION HANDLERS (3-5 items); "If prospect says: {objection} →
   Responses must reference a real user moat from winningSummary -
3. TALK TRACKS (2-3 items); 1-2 sentence opening pitches. Each leads
OUTPUT; write via a single heredoc to
- **{one-line fact}**; {how to use it in the call}. (source: {url})