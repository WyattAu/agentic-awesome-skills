A spec-and-scaffold skill for building composable recommendation, ranking, and feed pipelines. It encodes the six-stage **Source → Hydrator → Filter → Scorer → Selector → SideEffect** framework popularized by xAI's open-sourced [For You algorithm](https://github.com/xai-org/x-algorithm) (Apache 2.0). This skill is an independent reimplementation of the *pattern*; no code is copied from the original; licensed MIT. Use it whenever you need "the top K items for a (user, context)": social feeds, content CMSs, RAG rerankers, task prioritizers, notification triage, search reranking, ad ranking.
The upstream repository ships three runnable example scaffolds; every one green on its test suite:
- **Strapi v5 plugin** (TypeScript, Jest, 3/3 pass); adds `GET /api/feed/for-you` with multi-action scoring and author diversity
- **Zentra-compatible pipeline** (Go with generics, 3/3 pass); engine.Module-compatible, standalone-usable
- **PMAI task prioritizer** (Python / FastAPI / pytest, 3/3 pass); `GET /tasks/next?user_id=42&limit=10`
- ✅ Surface the multi-action vs single-score trade-off explicitly; don't default silently
- ❌ Don't invent benchmark or latency numbers; say "depends on workload, run it yourself"
- ❌ Don't name the user's generated artifact "X-like" or use "For You" branding; the pattern is free, the brand is not
- This skill scaffolds pipeline plumbing; it does not train ML models; the scoring function is the user's responsibility
- It does not choose infrastructure (vector DB, cache, queue); those are outside scope
- The generated scaffolds are framework code, not application logic; no shell commands, no network fetches, no credential handling
  **Solution:** Skill recommends multi-action prediction with tunable weights; change behavior by changing weights, no retraining