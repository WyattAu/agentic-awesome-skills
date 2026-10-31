# Inference Providers: when not to host the model
- Hardware can be `cpu-basic`; no GPU at all.
For those, host the model yourself on ZeroGPU; see [`zerogpu.md`](zerogpu.md).
### Mode A: Space creator pays (simple)
### Mode B: Visitor pays (recommended for public demos)
This is the **recommended pattern for public demos**; sustainable cost-wise, and visitors get to use their own provider quotas (which most have paid for or get free).
`cpu-basic`. No GPU. Don't put `--flavor zero-a10g`; you'd waste a paid grant.
If you do use Inference Providers, do **not** wrap the call in `@spaces.GPU`. The decorator reserves a GPU slot on your Space for the full `duration=`, but the function does no GPU work; just an HTTP call out. You burn your own ZeroGPU quota for nothing.