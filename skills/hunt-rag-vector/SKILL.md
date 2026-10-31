## LLM08: Vector & Embedding Weaknesses (RAG Pipeline Attacks)
`hunt-llm-ai` already owns *session-scoped* indirect injection; a hidden instruction in one
the vector **storage and retrieval layer itself**; attacks that don't need any prompt-injection
Read `hunt-llm-ai`'s False-Positive Gate first; it applies here unchanged (run-twice rule,
- API responses that include a "similar documents" or "sources" block; check whether it leaks
## Technique 1: Persistent Corpus Poisoning
unrelated query**; not just "the document I uploaded gets summarized when I open it again."
   [visible, on-topic filler about the app's actual subject matter; several paragraphs]
   topic; one that would not obviously retrieve *your specific* document by name.
   not persistent poisoning; it's the same session-scoped class `hunt-llm-ai` already owns.
## Technique 2: Cross-Tenant Vector-Store IDOR
DB is reachable directly; or if the app's query API accepts a document/namespace ID you can
manipulate; isolation may not hold at the layer that actually matters.
curl -s http://$TARGET:8000/api/v1/heartbeat                     # Chroma; confirms reachability
curl -s http://$TARGET:6333/collections                           # Qdrant; lists all collections, no auth check
corpus read; Critical on its own, no chaining required.
GET /api/knowledge/document/00042          # sequential/guessable ID; try 00041, 00043
independently verify belongs to a different, authorized test tenant/account; not merely "different-looking
## Technique 3: Source-Text / Metadata Leakage
- Check any `/similar`, `/search`, `/embeddings/query` endpoint for the same; these are
only realistic when you can also query the embedding model directly to build training pairs -
## Technique 4: Retrieval Hijack ("SEO Poisoning" for RAG)
topic. This is a lever, not a standalone finding; score it by what the LLM does with the
   clean session/account retrieving the payload via normal query flow; not a re-ask by the
2. **Verifiable cross-tenant artifact.** Same standard as `hunt-llm-ai`'s IDOR-via-AI; a value
   inversion"; they have different remediations (access control vs. output-layer redaction) and
- **`hunt-llm-ai`**; owns session-scoped prompt injection, exfil channels, and the base
- **`hunt-idor`**; vector-store cross-tenant leaks are IDOR at the retrieval layer; same
- **`hunt-api-misconfig`**; an exposed vector-DB admin API with no auth is the same underlying
- **`hunt-cloud-misconfig`**; managed vector-DB services (Pinecone, Weaviate Cloud) leak via
- **`triage-validation`**; enforce the False-Positive Gate before writing anything up;
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.