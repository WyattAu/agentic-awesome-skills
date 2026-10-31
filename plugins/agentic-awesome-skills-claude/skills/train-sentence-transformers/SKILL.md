**This SKILL.md is a router, not a manual.** It tells you which references and example scripts to load for your task. The actual content; recommended losses, evaluators, training-script structure, model selection, training-arg knobs, troubleshooting; lives in `references/` and `scripts/`.
### Per-type: always required
- `references/losses_sentence_transformer.md`; loss-to-data-shape mapping; `BatchSamplers.NO_DUPLICATES` requirement for MNRL-family; `Cached*` ↔ `gradient_checkpointing` incompatibility.
- `references/evaluators_sentence_transformer.md`; evaluator-to-task mapping; `metric_for_best_model` key construction (named vs unnamed); per-evaluator `primary_metric` values.
- `references/model_architectures.md`; encoder vs decoder vs static vs Router pipelines; pooling rules (mean / cls / lasttoken); auto-mean-pooling behavior for fresh-start MLM bases.
- `scripts/train_sentence_transformer_example.py`; production template; copy this as your starting point.
- `references/losses_cross_encoder.md`; pointwise / pairwise / listwise / distillation; `pos_weight` derivation; `activation_fn=Identity()` mandatory for non-BCE losses (silent eval-rank collapse otherwise).
- `references/evaluators_cross_encoder.md`; `CrossEncoderRerankingEvaluator` recipe; named-evaluator key format `eval_{name}_{primary_metric}`.
- `scripts/train_cross_encoder_example.py`; production template; copy this as your starting point.
- `references/losses_sparse_encoder.md`; `SpladeLoss` wrapper requirement; FLOPS regularizer weights; smoke-test active-dim ramp behavior.
- `references/evaluators_sparse_encoder.md`; `SparseNanoBEIREvaluator` (English-only) and the in-domain alternative; `eval_{name}_{primary_metric}` key format.
- `scripts/train_sparse_encoder_example.py`; production template; copy this as your starting point.
### Cross-cutting: always required (regardless of task)
- `references/training_args.md`; `TrainingArguments` knobs, precision rules (load fp32 + autocast bf16/fp16; never `torch_dtype=bfloat16`), `warmup_steps` (float) vs deprecated `warmup_ratio`, `save_steps` must be a multiple of `eval_steps` for `load_best_model_at_end`, schedulers, HPO, tracker, resume, hub-push variants.
- `references/dataset_formats.md`; column-matching rules (label name auto-detection; column-order-not-name); reshaping recipes; hard-negative mining options.
- `references/base_model_selection.md`; discovery commands; per-type model namespaces; ModernBERT-family `max_seq_length=8192` trap; `datasets >= 4` script-loader rejection; non-English starting-point shortcuts.
- `references/troubleshooting.md`; symptom-indexed failure recipes. Skim the section headings on every run, even a healthy one; the "Metrics don't improve" and "Hub push fails" entries cover bugs that bite frequently and are cheaper to recognize before they fire than to debug after.
### Cross-cutting: load when applicable
- `references/hardware_guide.md`; VRAM sizing, multi-GPU, FSDP / DeepSpeed, HF Jobs flavors. Required for >24GB models, multi-GPU, or HF Jobs runs.
- `references/hf_jobs_execution.md`; required when running on HF Jobs.
- `references/prompts_and_instructions.md`; required when using prompt-tuned bases (E5, BGE, GTE, Qwen3-Embedding, Instructor, Nomic, etc.) or adding `query: ` / `passage: ` style prefixes.
- Hard-negative mining CLI; `scripts/mine_hard_negatives.py`.