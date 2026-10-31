1. **Reduce `per_device_train_batch_size`**. Raise `gradient_accumulation_steps` to keep the effective batch size for regression losses. (For MNRL, effective batch via grad-accum is **not** equivalent; see point 3.)
   - `CachedMultipleNegativesRankingLoss(model, mini_batch_size=32)`; forwards in mini-batches, accumulates the contrastive loss over the full batch. Can simulate batch sizes of 1024+ on a 24GB GPU.
   - `CachedSpladeLoss(model, loss=..., mini_batch_size=16)`; same trick for sparse.
   - `CachedGISTEmbedLoss(model, guide_model, mini_batch_size=32)`; GIST variant.
For models >3B, use `accelerate config` to enable FSDP or DeepSpeed ZeRO. Both are supported; `sentence-transformers` doesn't require any code changes, only the launch config.
- **Evaluators don't run under FSDP** as of writing; the eval hooks call `model.encode()` which FSDP-wrapped modules can't service mid-training. Plan to evaluate post-training on a single-GPU load of the final checkpoint instead, or train with DDP if you need mid-training evaluation.
- **Slower than DDP** for models that fit on a single GPU; only reach for FSDP when you actually need the memory savings.
| RTX 4xxx, H100, B200 | `bf16=True` (or fp8 on H100 via specific kernels; not default) |