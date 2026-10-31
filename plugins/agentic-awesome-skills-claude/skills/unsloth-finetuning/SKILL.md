This skill covers **Unsloth Core**; the Python API; because that is what an agent can drive
- Use when a fine-tune "ran fine" but the model's output format is wrong; usually a chat
as planning figures and confirm against `nvidia-smi` on the first run; they vary by
capability. Setting `lora_alpha` to 1-2x `r` is a safe default. Keep `lora_dropout = 0.0` -
`2e-4` suits LoRA; full fine-tuning needs roughly 10x lower. One to three epochs is typical -
Avoid `save_method="merged_4bit"` for anything redistributed; it bakes in the quantization and
- ✅ Evaluate the exported artifact, not just the adapter; quantization shifts behaviour.
- Stop and ask for clarification if the GPU, model, dataset format or export target is unknown -
  outliers; peak memory tracks the longest sequence, not the mean.