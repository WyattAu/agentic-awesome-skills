- `mean` (default); average of token embeddings, masked to the attention mask. Strongest default.
- `cls`; embedding of the `[CLS]` token. Works if the base was CLS-pretrained.
- `max`; element-wise max across tokens. Rare.
- `mean_sqrt_len_tokens`; mean scaled by √seq_len. Empirically helps on some tasks.
- `weightedmean`; token-position-weighted mean. Useful for decoder bases as a non-last-token alternative.
- `lasttoken`; embedding of the last token. Required for causal-LM bases (see decoder section below).
Strong at long context, instruction following, multilingual. Memory-hungry; typically LoRA-trained rather than full fine-tuned.
**Why last-token pooling:** causal attention means only the last token has seen the full sequence. Mean-pooling a causal model averages embeddings that only saw prefixes; the result doesn't represent the whole input.
`StaticEmbedding` skips the transformer entirely; each token maps to a pre-computed vector via a lookup table. No attention, no contextualization.
- You have <100k training pairs; the model won't learn enough.
**Warm starts vs. random init**; with **>1M training samples**, random-init beats `StaticEmbedding.from_model2vec(...)` or `.from_distillation(...)` warm starts. With smaller datasets, warm starts help.
**Precision**: load in fp32 and pass `bf16=True` (or `fp16=True`) to TrainingArguments; autocast handles the inference path. Don't set `torch_dtype="bfloat16"` in `model_kwargs`: it puts Adam state in bf16 and silently degrades quality (see `training_args.md`).
**Warning**: Router-based models have unaligned embedding spaces at init; you must train to align them. Use a `Dense` projection layer when dimensions differ. Task-based routing (different encoders for queries vs. documents) is also supported via `route_mappings`; see the `Router` docstring.
- **Large VLM backbones on consumer GPUs**: combine LoRA + `attn_implementation="flash_attention_2"`. With LoRA only, you can additionally pass `torch_dtype="bfloat16"`; the bf16 base weights are frozen, so the Adam-state precision concern from the precision rule above doesn't apply (the LoRA adapter stays fp32, so its optimizer state stays fp32). Without LoRA, follow the precision rule: keep weights fp32 and rely on `bf16=True` autocast.