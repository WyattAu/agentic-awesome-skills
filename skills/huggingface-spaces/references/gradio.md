For deeper Gradio API guidance; components, layouts, event listeners, chatbots, the Gradio 5→6 migration; use the dedicated `huggingface-gradio` skill. Install it with `hf skills add huggingface-gradio` (add `--claude --global` to also install for Claude Code, user-level).
- Width-cap with `!important` if Gradio 6's own breakpoints fight you; target `main`, `.gradio-container`, and the inner fillable wrapper, otherwise the width caps but goes flush-left.
Be succinct. Title, one-line description, a links section. Don't over-explain how it works; that's what the model card is for.
- **`cache_mode="lazy"`**; caches on first user click for each example. **Required on ZeroGPU** (Gradio's default on ZeroGPU is already lazy via `GRADIO_CACHE_MODE=lazy`). Eager would pre-run every example at app startup, but ZeroGPU has no GPU attached at startup; it'd fail and burn the creator's daily quota.
Use `gr.update(label=...)` for status narration; feels like a status line without a separate component.
**Caveat**: `gr.Progress(track_tqdm=True)` and `yield` partial outputs fight each other; pick one streaming mechanism.
## Custom frontends: `gr.Server`
`launch(ssr_mode=False)` is ignored on HF; must be the env var.
**Don't stack `@spaces.GPU` and `@app.api`** on the same function; silently breaks request flow. Keep them on separate functions.
**Two-ceiling coordination**: both `@spaces.GPU(duration=N)` and `@app.api(time_limit=M)` apply; the lower wins. Set `time_limit` to your max duration across modes; a too-low `time_limit` kills the request even if GPU duration would have allowed it.
Hot-reload (rung 1 in [`debugging.md`](debugging.md)) does **not** work with `gr.Server`; always `hf upload` or commit.
- `gr.Button(text="X")`; `text=` was removed; use `gr.Button("X")`.
- `gr.Button(type="button")`; drop the kwarg.
- `.click(..., _js=share_js)`; renamed to `js=`.
- `.style(height=...)`; removed in gradio 4+.
- `gr.ImageMask(brush_color=...)`; kwarg removed.
- `demo.launch(mcp_server=True)` on gradio 4.x; only valid on 5+.