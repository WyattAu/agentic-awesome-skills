- **Main web process**; long-lived. Imports `app.py`, launches Gradio. Holds no VRAM and, after the startup "pack" step, no model weights in RAM either.
- **GPU worker**; short-lived. Forked per `@spaces.GPU` request (or reused if warm). Eventually killed by the ZeroGPU scheduler when another Space needs the slot. Your code never kills its own worker.
- **Cold worker**; forked from the main process; torch is unpatched; real CUDA is initialized; weights are streamed disk → pinned host → VRAM via a double-buffered pipeline. This is the cold-start cost.
- **Warm worker**; alive worker bound to the same slot; init is skipped; weights stay on VRAM from the previous call.
Use the **string `"cuda"`**; never an integer device id. ZeroGPU re-allocates device ids per request, so `.to(0)`, `device_map={"": 0}`, `torch.cuda.set_device(0)` silently break.
For plain `from_pretrained` loads, use `.to("cuda")`, **not** `device_map="cuda"` (which routes through `accelerate.set_module_tensor_to_device` and calls `torch._C._cuda_init()` at load time, bypassing the hijack). The exception is loaders that are ZeroGPU-aware; notably the `bitsandbytes` quantization path; `from_pretrained(..., quantization_config=BitsAndBytesConfig(...))` works with `device_map="cuda"`.
**Preloading multiple variants** (e.g. base + refiner, image + video model) is fine as long as their combined VRAM fits. Load all of them sequentially at module scope into a dict, then key per request. Don't unload/reload between requests; that puts the load cost back on the user.
- **`ZeroGPU illegal duration`**; `N` exceeds the visitor's tier cap. Lowering `duration` is the only fix.
- **`ZeroGPU quota exceeded`**; the visitor's remaining quota is less than `requested`. Compared as `requested vs remaining`, not `actual vs remaining`; so a 10-second task left at the default 60 s blocks the user as soon as their remaining drops below 60 s.
**Pick the value; don't guess.** A too-high duration deploys cleanly then errors on the first call; too-low silently truncates. Methodology:
    # Swallow extras with *args, **kwargs: Gradio passes progress= positionally
`size="large"` (default) is half the backing card (48 GB on Blackwell). `size="xlarge"` is the full card (96 GB) and costs **2× quota** per second; plus higher queue waits. Use `large` unless the workload genuinely OOMs.
    device_map="cuda",            # OK here; bnb's loader is ZeroGPU-aware
`load_in_8bit=True` swaps the 4-bit block for int8; same hijack-safe loader. Bigger but higher quality, no `compute_dtype` knob.
`torchao` is more flexible (fine-grained per-module quantization, `Int4WeightOnlyConfig`, `Float8WeightOnlyConfig`, etc.) and works with diffusers' `from_pretrained(..., quantization_config=TorchAoConfig(...))` integration too. No CUDA build dependency; installs as a wheel.
Default to `attn_implementation="sdpa"`; torch-native, works everywhere. Reach for an FA backend only when the upstream repo uses it by default or strongly recommends it. If it breaks on Blackwell, fall back to SDPA.
**xformers** is available via the prebuilt wheel; auto-dispatch picks FA2 on sm_120 with no monkey-patch.
3. **Read-only globals are safe**; models, tokenizers, configs loaded once and only read inside handlers.
- `gr.SelectData` is a special case; its `__getattr__` recurses under pickle. Extract the fields you need (`evt.index[0]`, etc.) in a thin un-decorated wrapper, pass plain values to the `@spaces.GPU` function.
- Yielding `gr.update()` for a state slot **skips** the update; other handlers continue to see pre-yield value.
- For large state, minimize how often you yield it; ideally once at the end.
`@spaces.GPU` supports generator functions; first-class for progressive UI updates:
`gr.Progress(track_tqdm=True)` and `yield` compete with each other; pick one.
**Do not** use `ProcessPoolExecutor` / `multiprocessing.Pool` inside `@spaces.GPU`; the daemonic fork can't spawn children (`AssertionError: daemonic processes are not allowed to have children`). Threads only.
**Do NOT** wrap `import spaces` in `try/except` with a no-op fallback. Off-ZeroGPU, the `spaces` package is *already* a true no-op; the heavyweight behavior is gated on `SPACES_ZERO_GPU=1`, set only on ZeroGPU. `@spaces.GPU` returns the undecorated function unchanged elsewhere. The Gradio base image installs `spaces` on every hardware tier, so a duplicate onto T4 / A10G / CPU works without code changes too.
That said: **iterate ON the Space, not locally.** The Space environment (Python, torch, CUDA, drivers, env vars) differs from yours; passing local tests doesn't prove the Space works. Run useful local tests, then deploy a staging candidate within the agreed scope; and use the rung ladder ([`debugging.md`](debugging.md)) against the live URL.
Don't override to `cache_mode="eager"` on ZeroGPU; it will fail or burn the creator's daily quota. The cache is keyed by example **file path**, not content hash: regenerating an asset in place serves the stale cached output. Bump a `cache_version` constant if you replace example files.
For real-time apps (webcam, audio streaming), the per-call fork model is too costly. ZeroGPU supports reusable "real-time sessions"; one GPU allocation amortized across many small requests. Reference Spaces:
When the log endpoint can't explain a failure (device-side asserts, OOM under specific shapes, race conditions in pickle), dev mode + SSH is the last-resort tool; see [`debugging.md`](debugging.md).