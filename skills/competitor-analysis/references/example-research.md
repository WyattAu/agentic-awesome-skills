- [Template](#template); full worked example for a fictional "Rival Co"
- [Field Rules](#field-rules); frontmatter fields, body section order, mention/findings format
- [Writing via Bash Heredoc](#writing-via-bash-heredoc); required pattern for subagents to avoid permission prompts
- **[Benchmark]** retrieval-quality leaderboard; Rival Co 73% nDCG@10, 4th of 7 tested (source: https://github.com/example-org/search-bench/pull/92, 2026-03-14)
- **[Comparison]** Exa vs Rival Co; side-by-side review (source: https://example.com/exa-vs-rivalco, 2026-02-01)
- **[Reddit]** r/LangChain thread: "Moved from Rival Co to X after relevance issues"; 24 upvotes (source: https://reddit.com/r/LangChain/comments/abc123)
- **[HN]** "Show HN: Rival Co raises seed to build..."; 112 points, 48 comments (source: https://news.ycombinator.com/item?id=12345)
- **[LinkedIn]** CEO post on product launch; 412 reactions (source: https://linkedin.com/posts/rivalco-launch)
- **[YouTube]** "Rival Co vs Exa" review by Dev YouTuber; 8.2K views (source: https://youtube.com/watch?v=xyz)
- **search-bench PR #92**; Rival Co 73% nDCG@10 on retrieval quality, 4th of 7 tested (https://github.com/example-org/search-bench/pull/92)
- **retrieval-latency blog**; Rival Co 480ms p50, 2nd fastest (https://example.com/search-latency-2026)
- **Rival Co scores 73% nDCG@10 on the search-bench leaderboard (4th of 7 tested)**; use against relevance-sensitive prospects; they rank below Exa on the same test. (source: https://github.com/example-org/search-bench/pull/92)
- **G2 average 4.3/5 with "stale results" as top complaint across 31 reviews**; cite when prospect raises freshness concerns. (source: https://g2.com/products/rival-co)
- If they say: "Rival Co is $99/mo; cheaper than your Pro tier"
  You say: "Cheaper upfront, but compare total cost of poor relevance; their 73% nDCG@10 means more irrelevant results your agent has to filter or re-query, and re-queries aren't free." (evidence: https://github.com/example-org/search-bench/pull/92)
- **Mentions format**: `- **[SourceType]** title | snippet (source: url, date)`; `SourceType` is one of `Benchmark`, `Comparison`, `News`, `Reddit`, `HN`, `LinkedIn`, `YouTube`, `Review`, `Podcast`, `X`.
- **Findings format**: `- **[confidence]** fact (source: url)`; `confidence` is `high`, `medium`, or `low`.
Subagents write these files using bash heredoc to avoid security prompts. Use the full literal `{OUTPUT_DIR}` path; no `~` or `$HOME`: