| Rerank retrieval results (nDCG@k on BM25 top-N); fast default | `CrossEncoderNanoBEIREvaluator` |
- `eval_NanoBEIR_R100_mean_ndcg@10`; `CrossEncoderNanoBEIREvaluator` default
- `eval_{name}_ndcg@10`; `CrossEncoderRerankingEvaluator`
- `eval_{name}_average_precision`; `CrossEncoderClassificationEvaluator` (binary, `num_labels=1`)
- `eval_{name}_f1_macro`; `CrossEncoderClassificationEvaluator` (multi-class, `num_labels>=2`)
- `eval_{name}_spearman`; `CrossEncoderCorrelationEvaluator`
- **Always run `evaluator(model)` once before training**; pre-training baseline. A tiny delta may reflect uncertainty, a strong baseline, insufficient compute or task mismatch; investigate rather than declaring failure.