# Competitor Analysis: Research Patterns
- [Overview](#overview); two research contexts (self vs target)
- [Self-Research (User's Company)](#self-research-users-company); sub-questions, page discovery, synthesis output (precise_category, include keywords, exclusion list)
- [Competitor Research; 4 Research Lanes](#competitor-research--4-research-lanes); Marketing / External / Benchmarks / Strategic Diff
- [Depth Mode Behavior](#depth-mode-behavior); quick / deep / deeper budgets and scope
- [Finding Format (per lane)](#finding-format-per-lane); JSON shape, confidence levels
- [Research Loop Rules](#research-loop-rules); 7 meta-rules for the research phase
- [Synthesis Instructions](#synthesis-instructions); turn findings into matrix cells
1. **Self-Research** (Step 1); Deep research on the user's company so we know what "competitor" means for this run.
2. **Competitor Research** (Step 4); For each discovered/seeded competitor, run the 4-lane enrichment below.
Dynamic via sitemap; do NOT hardcode `/about` or `/pricing`:
1. `browse cloud fetch "{company website}/sitemap.xml"`; primary source
- **precise_category**; one clear sentence that describes what category this product competes in. Avoid fuzzy words like "tools" or "platform". Good: "AI web search API for agents with neural + keyword retrieval". Bad: "search tools". This becomes the anchor for discovery queries and the gate.
- **category_include_keywords**; 8-15 phrases that a *direct competitor's* marketing would very likely contain (title or hero). Include semantic variants. e.g. for Exa: `web search api`, `search api`, `neural search`, `semantic search`, `retrieval api`, `search for ai agents`, `search for llms`, `serp api`, `embeddings search`, `live crawling`, `answer api`, `research api`.
- **exclusion_list**; phrases that indicate a *different* category, used by the gate to reject false positives. e.g. `vector database`, `enterprise search appliance`, `site search widget`, `observability`, `analytics platform`, `data warehouse`, `scraping platform` (full ETL/scraping suites, not retrieval APIs), `internal knowledge base`.
## Competitor Research: 4 Research Lanes
### Lane 1: Marketing Surface (ALL depth modes)
**Pages to fetch** (via sitemap discovery; do NOT hardcode):
### Lane 2: External Signal (deep + deeper)
For LinkedIn and YouTube, the snippet + URL from `browse cloud search` is enough. Do NOT try to deep-fetch individual LinkedIn posts (auth walls); list them with title/snippet.
### Lane 3: Public Benchmarks (deeper only)
### Lane 4: Strategic Diff vs User's Company (deeper only)
**Inputs**: `{user_company_profile}` (from Step 1); specifically `product`, `use_cases`, `key_features` if available.
**No new fetches required** for this lane; it's a synthesis step over Lane 1 + 2 + 3 findings plus the user's profile. Write as:
1. **Lane 1 first**; always start with the competitor's own site
2. **Use sitemap, not hardcoded paths**; `/pricing` might be `/plans` or `/pricing-plans`
3. **Rephrase, don't retry**; if a search returns generic junk, switch keywords
4. **Fetch selectively**; pick the 1-2 most promising URLs per query
5. **For LinkedIn/YouTube: search only, don't fetch**; snippet is enough, avoid auth walls
7. **Deduplicate mentions**; same URL should only appear once in `## Mentions`