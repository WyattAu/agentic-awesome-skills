| `(anchor, positive, negative)` triplets | `MultipleNegativesRankingLoss`; it handles triplets natively |
- **Tip**: batch size matters a lot; more in-batch negatives = better gradients.
MNRL computed bidirectionally; scores positives from both (anchor -> positive) and (positive -> anchor) directions. Slightly better on retrieval tasks where the "anchor" and "positive" distinctions are soft (paraphrase, deduplication).
Classic triplet margin loss on explicit `(anchor, positive, negative)`. Uses a fixed margin and the hardest in-batch is not considered; only the provided triplet.
These don't have their own data shape; they wrap another loss and add a regularization objective.
Sentence-level denoising autoencoder: corrupt a sentence (drop tokens), force the model to reconstruct it. Pretraining-style; useful for domain adaptation when you have unlabeled in-domain sentences.