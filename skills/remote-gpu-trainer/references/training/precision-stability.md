# Numerical precision & training stability: make it RUN, then stop it diverging
NOT own *is the converged number real* / cuDNN-nondeterminism-as-a-metric-error; that is
- **Precision choice**; P1 fp32/tf32/fp16/bf16 decision · P2 TF32 default-off footgun · P3 H100/A100/V100 capability
- **AMP mechanics**; P4 autocast scope · P5 GradScaler (fp16 only) · P6 bf16 needs no scaler · P7 grad-clip under scaler
- **NaN / Inf**; P8 where NaNs come from · P9 anomaly detection · P10 fp16 overflow vs underflow · P11 bad-data NaN
- **Loss spikes / divergence**; P12 LR + warmup · P13 grad clipping · P14 skip-the-batch · P15 z-loss · P16 qk-norm · P17 init
- **Gradients**; P18 explosion/vanishing diagnosis
- **Repro**; P19 determinism knobs (cross-link)
- **Pointers**; gotchas_universal.md, multinode.md, spot-resilience.md
### P1: Which precision: fp32 / TF32 / fp16 / bf16
**Fix; default ladder (PyTorch 2.x)**:
1. **bf16 autocast** on Ampere+ (A100/H100/4090/...); the modern default; same range as fp32, no GradScaler, robust. `torch.autocast("cuda", dtype=torch.bfloat16)`.
2. **TF32** for the fp32 matmuls that remain (the non-autocast path); `torch.set_float32_matmul_precision("high")`. Free ~speedup, negligible convergence impact for most nets (P2).
3. **fp16 autocast + GradScaler** ONLY if stuck on a card with no bf16 tensor cores (V100/T4/2080Ti); needs the scaler (P5) and is overflow-prone.
### P2: TF32 is OFF by default for matmul since PyTorch 1.12: the "why is my A100 slow" footgun
**Fix**: opt back in once at startup -
### P3: Card capability gates the choice: bf16 needs Ampere+; V100/T4 are fp16-only
**Fix**: branch on capability at runtime, never hardcode -
### P4: autocast: wrap ONLY forward + loss, never backward, never `.half()` the model
...)` / `torch.amp.GradScaler("cuda")` API; `torch.cuda.amp.*` is **deprecated** in PyTorch 2.x. autocast
### P5: GradScaler: required for fp16 to stop gradient *underflow*
**Symptom (no scaler, fp16)**: loss looks fine but the model doesn't learn; small gradients flush to 0 in
**Fix; canonical fp16 loop**:
### P6: bf16 needs NO GradScaler (adding one is pointless, not harmful)
**Symptom**: a copied fp16 recipe carries a GradScaler into a bf16 run; wasted overhead, not a crash or a wrong result.
**Fix**: for bf16, drop the scaler entirely; plain `loss.backward(); optimizer.step()`. Only fp16 (and the
### P7: Gradient clipping under GradScaler: `unscale_` FIRST or you clip scaled grads
Call `unscale_` at most once per optimizer per step; repeated calls can raise an error. For bf16, just `clip_grad_norm_` directly; no unscale.
### P8: Where NaNs come from: the four arithmetic origins
**Root cause**; NaN/Inf is produced by a *finite* set of ops on bad inputs:
**Fix; make the op stable, don't paper over it**:
- Never hand-roll `log(softmax(x))`; use `F.log_softmax` / `F.cross_entropy` (fused, log-sum-exp-stable).
### P9: Find the exact op: anomaly detection + a cheap forward hook
**Fix; two tools, cheap → precise**:
- **Forward NaN hook (debug only; device reductions and Python branching can synchronize and slow training)**; register on every module to catch the *first* layer to emit NaN:
- **`torch.autograd.set_detect_anomaly(True)` (expensive, debug-only)**; records the forward traceback of
  The docs warn it "will slow down your program" (roughly an order of magnitude); enable to *locate*, then
### P10: fp16 overflow vs underflow: read the GradScaler signal
**Fix**: switch fp16 → **bf16** (P1); its fp32 range absorbs the large values; this is the single most
### P11: NaN from the *data*, not the math
**Root cause**: a corrupt sample; NaN/Inf pixel, all-zero target, label outside `[0, C)`, empty sequence,
**Fix**: guard at the data boundary; `assert torch.isfinite(x).all(), f"non-finite input @ step {step}"`
index); a *step-varying* NaN ⇒ a numerics/LR problem (P12), not data. Smoke the data first; smoke
### P12: Loss spike / divergence: LR too high or warmup too short
sometimes diverging to NaN; most often early, or after a fast LR ramp.
**Fix; in order of cheapness**:
3. **Log grad-norm every step** as the early-warning signal; spikes are predictable from activation/grad-norm scaling before they hit.
### P13: Gradient clipping: the standard guardrail (and what constant clipping means)
**Fix**: clip global grad norm every step; `torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm)`
(LR too high P12, bad init P17, architecture), not a fix; chase the cause. Global-norm clipping scales
*all* grads down, so one embedding-heavy batch can throttle everything else that step; consider per-module
### P14: Skip-the-batch: drop the update when this step is non-finite
For distributed training, reduce an agreed non-finite flag across all participating ranks before the update so all ranks step or skip together; do not let a rank skip required collectives. Check buffers and optimizer state too, and record every skipped sample. Log a `skipped` counter; a *rising* skip rate means a systematic problem (P12/P10), not stray bad data.
### P15: z-loss: stop softmax logits from drifting unbounded
### P16: qk-norm: kill attention-logit growth at high LR
**Root cause**: "growth of logits in attention layers"; one of the two dominant transformer instability
**Fix**: apply **QK-LayerNorm**; LayerNorm query and key per-head before the dot-product. Combined with
### P17: Initialization & normalization placement
### P18: Gradient explosion vs vanishing: diagnose by logging the norm
**Fix; measure first**:
A grad-norm trace is the cheapest, highest-signal stability instrument; log it from step 1.
### P19: Deterministic / repro knobs: set them, but the *interpretation* is delegated
**Fix; the mechanical knobs (set these here)**:
methodology, is owned by verifying-dl-experiments (cross-link REQUIRED)**; catalogued as **U36** in
speed; enable for the datapoint that must be clean, not every throwaway run.
## Pointers: adjacent layers, do NOT restate here
- **`references/gotchas_universal.md`**; the *infra* failure modes that masquerade as numerics:
- **`verifying-dl-experiments`** (**REQUIRED** cross-link); owns *is-the-number-real*: smoke **content**,
- **`references/spot-resilience.md`**; checkpoint cadence so a divergence-and-resume (P12) loses minimal work.
- **`references/multinode.md`**; NCCL/precision interactions in DDP (all-reduce dtype, loss-scale sync) for