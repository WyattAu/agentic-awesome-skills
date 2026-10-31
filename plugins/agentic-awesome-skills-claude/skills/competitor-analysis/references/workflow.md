# Competitor Analysis: Workflow Reference
- [Discovery Batch JSON Schema](#discovery-batch-json-schema); browse cloud search output format
- [Competitor Research Markdown Format](#competitor-research-markdown-format); frontmatter + body section spec
- [Extracting Page Text](#extracting-page-text); browse cloud fetch (markdown default; --format raw for HTML)
- [Discovery; parallel Bash, not subagents](#discovery--parallel-bash-not-subagents); Wave A/B/C recipes
- [Enrichment fan-out; 5 subagents PER competitor](#enrichment-fan-out--5-subagents-per-competitor-deepdeeper-modes)
- [Legacy: Single-subagent template](#legacy-single-subagent-template-quick-mode-only); quick mode only
- [Wave Management](#wave-management); parallelism rule, gate phase, sizing formula
- [Report Compilation](#report-compilation); compile_report.mjs invocation
File: `{OUTPUT_DIR}/{competitor-slug}.md`; see `references/example-research.md` for the full template.
**Body sections** (in this order; `compile_report.mjs` parses by heading):
`browse cloud fetch --allow-redirects` returns clean **markdown by default**; no HTML stripping needed. Just cap the length:
## Discovery: parallel Bash, not subagents
Example; main agent issues these three Bash tool calls in parallel in one message:
# Wave A: alternatives
# Wave B: precise category
# Wave C: comparison-page graph
Why direct Bash and not subagents: each wave is 2-4 `browse cloud search` calls; agent cold-start + tool-reasoning overhead is bigger than the actual work. Using parallel Bash saves ~1-2 min per run with no quality loss.
Discovery uses **three parallel waves** (evaluated; all three are additive):
**Wave A; Generic alternatives** (broad net, lots of noise):
**Wave B; Precise category queries** (uses `precise_category` from self-research):
- `"{precise_category_2_3_keywords}"`; pick the 3 most distinctive tokens
**Wave C; Comparison-page graph** (highest-precision single wave):
## Enrichment fan-out: 5 subagents PER competitor (deep/deeper modes)
| **B. Discussion** | `discussion` | Reddit, HN, forums, dev.to, hashnode. Broader queries beyond `site:` restrictions; also `"{competitor}" discussion`, `"{competitor}" review 2026`, `"{competitor}" issues OR problems`. Writes Mentions bullets with dates. |
| **C. Social** | `social` | LinkedIn posts, YouTube videos, Twitter/X threads. Search snippets only; do NOT fetch (auth walls). |
**Wave management; launch ALL subagents in ONE message**: for N competitors × 5 lanes = 5N subagents, fit them all in a single Agent-tool message. Wall clock then equals the slowest single subagent (~3-5 min) instead of `batches × slowest_per_batch`. On a real 10-competitor run we measured 25 minutes wasted by self-throttling to 10-per-message; the Agent tool happily runs 50+ in parallel; do not split into batches for "politeness". The only cap is that each subagent still batches its own Bash operations into a single call.
In `quick` mode, keep a single subagent per batch of competitors (no fan-out; Lane 1 only, budget 2-3 calls each).
TOOL RULES; CRITICAL, FOLLOW EXACTLY:
   browse cloud fetch returns clean markdown by default; just `| head -c 3000`, no HTML stripping.
5. BANNED TOOLS: WebFetch, WebSearch, Write, Read, Glob, Grep; ALL BANNED.
6. NEVER use ~ or $HOME in paths; use full literal paths.
RESEARCH PATTERN (per competitor; lanes are depth-gated):
LANE 1; Marketing Surface (always run):
  b. Discover via sitemap: /sitemap.xml; find /pricing, /features, /integrations, /customers
LANE 2; External Signal (deep + deeper):
    twitter.com/x.com → X (or Twitter; either works)
  For LinkedIn and YouTube; use search snippet only, do NOT fetch the page.
LANE 3; Public Benchmarks (deeper only):
LANE 4; Strategic Diff vs {user_company} (deeper only):
HARD TOOL-CALL CAP; count your browse cloud calls and STOP at the cap. Partial output beats blocking the pipeline.
ENFORCEMENT; at the start of every Bash call, prepend a comment like
After hitting the cap, write the output file with WHAT YOU HAVE; even if a section is thin.
OUTPUT; write ALL competitor files in a SINGLE Bash call using chained heredocs directly to {OUTPUT_DIR}:
strategic_diff: {one line; deeper only}
**Launch ALL subagents needed for a phase in ONE message.** No "up to 6 per message" cap; the Agent tool runs them in parallel, so wall clock = slowest single agent regardless of count. On a 10-competitor × 5-lane = 50-subagent enrichment, splitting into 5 batches of 10 cost an extra 20 minutes of wall clock vs one batch of 50 (measured Apr 2026). Each subagent still MUST batch its own Bash operations into a single call.
- **Run discovery as parallel `browse cloud search` Bash calls, not subagents.** Subagent overhead (cold start + tool reasoning) is bigger than the work. Three Bash tool calls in one message; one per wave (A/B/C); chain each wave's searches with `&&`.
### User-confirm phase (between gate and enrichment: mandatory)
After the gate writes `/tmp/competitor_gated.jsonl`, the main agent MUST ask the user to confirm the enrichment set before launching subagents. Enrichment is 25 subagents × depth budget per competitor; too expensive to run on guesses.
1. **PASS**; status=PASS rows with title
2. **UNKNOWN**; status=UNKNOWN (fetch failed; always a silent miss risk; JS-heavy homepages, Cloudflare challenges)
3. **Rejected-brand matches**; top ~10 REJECT rows whose title contains a seed token or that showed up repeatedly in the Wave C "X vs Y" graph
Then `AskUserQuestion` with a checkbox list + free-text "add more". Write the confirmed set to `/tmp/competitor_enrichment_set.txt` (one URL per line). That file; not `/tmp/competitor_passed.txt`; is the input to the enrichment subagents.
- "Search foundation" / "retrieval backbone" / "agent runtime"; semantic variants of the category don't lexically match
**Review the output**; the main agent SHOULD spot-check both lists and MAY manually re-include a REJECT if it recognizes a known direct competitor whose own marketing is category-ambiguous.
**Evaluation on a search-API run** (12 candidates): 7/7 real competitors PASSED; 4/4 wrong-category (vector database, scraping/ETL platform, analytics tool, internal-KB search) REJECTED. One split-identity edge (a search vendor that also sells a scraping suite) rejected; acceptable.
- **`quick` mode**; single subagent per batch of competitors. Lane A (marketing) only. ~8 competitors per subagent, 2-3 tool calls each. Writes directly to `{OUTPUT_DIR}/{slug}.md`.
- **`deep` / `deeper` modes**; 5-subagent fan-out PER competitor. Each subagent owns ONE lane (marketing / discussion / social / news / technical). Writes to `{OUTPUT_DIR}/partials/{slug}.{lane}.md`. Budget: 5-8 calls per subagent (deep), 10-15 (deeper). After all lanes complete, run `scripts/merge_partials.mjs` to consolidate.
- **Launch ALL competitor × lane subagents in a SINGLE Agent tool message.** For 10 competitors × 5 lanes = 50 parallel agents in one message. Do NOT split into batches; wall clock becomes the slowest single agent (~3-5 min) instead of batches-times-batch-max (~25 min on 10 competitors split into 5 rounds of 10).
- Generates `{OUTPUT_DIR}/index.html`; overview table (name, tagline, pricing, key features, strategic diff)
- Generates `{OUTPUT_DIR}/competitors/{slug}.html`; per-competitor deep dive
- Generates `{OUTPUT_DIR}/matrix.html`; side-by-side feature/pricing grid across competitors
- Generates `{OUTPUT_DIR}/mentions.html`; chronological feed with source-type pills + client-side filter
- Generates `{OUTPUT_DIR}/results.csv`; flat spreadsheet