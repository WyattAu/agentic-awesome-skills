| Retrieval (nDCG, MRR, Recall); fast default | `NanoBEIREvaluator` |
Heavy for large corpora; each eval encodes the full corpus. Don't run it every 100 steps. Use `NanoBEIREvaluator` for frequent evaluation during training and reserve full IR for milestones / post-training.
For labeled pair classification (e.g. duplicate detection, entailment as binary). Reports accuracy, F1, precision/recall, AP. Supports all distance metrics; finds the best threshold per metric.
- `eval_NanoBEIR_mean_cosine_ndcg@10`; `NanoBEIREvaluator`
- `eval_sts-dev_spearman_cosine`; `EmbeddingSimilarityEvaluator(name="sts-dev")`
- `eval_{name}_cosine_ndcg@10`; `InformationRetrievalEvaluator(name=...)`
- **Always run `evaluator(model)` once before training**; pre-training baseline. A tiny delta alone does not establish a broken configuration.
- Don't run `InformationRetrievalEvaluator` with a large corpus (>100k docs) at frequent `eval_steps`; use `NanoBEIREvaluator` during training, reserve full IR for end-of-training.