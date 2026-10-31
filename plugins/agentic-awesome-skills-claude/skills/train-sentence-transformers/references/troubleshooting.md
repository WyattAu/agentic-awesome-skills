3. Check `metric_for_best_model`; if the key doesn't match what the evaluator writes, the trainer may raise a missing-metric error; inspect actual keys and stop on mismatch.
- Trackio: confirm `pip install trackio` succeeded. No login step; trackio uses your `HF_TOKEN` (set by `hf auth login` or the `HF_TOKEN` env var). On HF Jobs, `HF_TOKEN` must be in `secrets`.
**Root cause:** the default `Sigmoid` activation function on `CrossEncoder(num_labels=1, ...)` saturates raw logits >5 to ~1.0. Distillation/listwise/pairwise losses (`MSELoss`, `MarginMSELoss`, `LambdaLoss`, `RankNetLoss`, `ListNetLoss`, `ListMLELoss`, `PListMLELoss`, `ADRMSELoss`) operate on raw logits; once the model learns to push positives past the saturation point, ranking information is lost.
1. **Lower `mini_batch_size`** on the loss first. The internal forward chunking preserves the K-list semantic; this is the cheapest knob and doesn't change the experiment.
For very large K (>=128), `NDCGLoss2PPScheme` materializes O(K²) weight buffers per query that aren't covered by the forward chunking, so even small `mini_batch_size` may not be enough; at that point K is the right knob.
**Root cause:** when a custom `nn.Module` is defined inline in a script, Python records its qualname as `__main__.ClassifierHead`. ST writes that qualname into `modules.json` at save time. Loading from a different entry point can't resolve `__main__.ClassifierHead`; the class doesn't exist in the loader's `__main__`.
2. Build the same shape using stock ST modules (`Dense + LayerNorm + Dense`) instead of a custom class; those are always loadable.
- For genuine CPU inference (no GPU available), consider switching to a `StaticEmbedding`-based model; ~1000x faster on CPU than a transformer.
- `training_args.md` (shared); the args that affect all of the above.
- `hardware_guide.md` (shared); VRAM sizing and multi-GPU.
- `dataset_formats.md` (shared); column/loss validation.
- `losses_sentence_transformer.md` / `losses_cross_encoder.md` / `losses_sparse_encoder.md` (per-model-type catalogs); loss-specific quirks.