| Retrieval (nDCG, MRR, Recall); fast default | `SparseNanoBEIREvaluator` |
- `query_active_dims`; non-zero entries per query vector
- `document_active_dims`; non-zero entries per document vector
A healthy SPLADE checkpoint typically shows ~30–50 active dims for queries and ~150–250 for documents. If these drift toward the vocab size (~30k), the FLOPS regularization isn't doing its job; raise `query_regularizer_weight` / `document_regularizer_weight` in `SpladeLoss`.
For `(anchor, positive, negative)` triplets; reports fraction where the positive is closer than the negative (by dot product).
- `eval_NanoBEIR_mean_dot_ndcg@10`; `SparseNanoBEIREvaluator` default
- `eval_{name}_dot_ndcg@10`; `SparseInformationRetrievalEvaluator`
- `eval_{name}_spearman_dot`; `SparseEmbeddingSimilarityEvaluator`
- **Always run `evaluator(model)` once before training**; confirms the pipeline works (a fill-mask base scores ~0 on retrieval until trained).
- Always check `query_active_dims` / `document_active_dims`; thousands of active dims per doc means the FLOPS regularizer is mistuned, even if nDCG looks OK.