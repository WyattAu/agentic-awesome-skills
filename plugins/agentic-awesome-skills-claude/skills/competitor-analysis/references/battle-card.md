# Battle Card: format spec
The Battle lane is the **6th** subagent lane in deep/deeper mode. It runs AFTER Step 5c fact-check completes; it reads only existing partials + the fact-checked `matrix.json`, **never makes new `browse cloud` calls**. This is a pure synthesis lane.
Concrete, verifiable facts about the competitor that **hurt them in a deal**. Every item must cite a URL from an existing partial (Mentions, Benchmarks, or Research Findings). Prefer third-party evidence (benchmarks, reviews, news) over the competitor's own marketing; marketing claims are weak ammunition.
- **{one-line factual claim}**; {how an AE uses it in the call}. (source: {url})
- **Rival Co placed 4th of 7 on the Nov 2025 search-bench retrieval leaderboard (73% nDCG@10)**; use if prospect cares about relevance, but only after confirming their volume tier; Rival Co's reranking add-on is paywalled behind Scale ($499/mo). (source: https://github.com/example-org/search-bench)
  You say: "Rival Co's reranking is a paid add-on you'll need for production relevance; once you add it the price gap closes. Our Scale tier includes neural reranking and a research endpoint; matrix.json confirms Rival Co's feature set doesn't cover the research API." (evidence: https://docs.rivalco.com/changelog)
1. For production RAG, Exa is the only provider in the category with BOTH a first-party neural index AND a dedicated research/answer endpoint; Rival Co shipped neither, Serper shipped neither, and one competitor replaced its answer endpoint with a thin LLM wrapper last quarter.
- **Fact 1**; usage. (source: url)
- **Fact 2**; usage. (source: url)
## Quality gates: Adversarial self-check (subagent MUST run before writing)
- [ ] Objections are realistic; they're what a prospect would actually raise based on the competitor's strongest marketing lines, not strawmen.