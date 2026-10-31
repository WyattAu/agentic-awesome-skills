Applies cleanly to function-body changes and new top-level symbols. Does **not** rerun module-level imports or one-time init; the model was loaded when `app.py` first ran and jurigged won't re-execute that load. New pip deps, README frontmatter, `Dockerfile`, and hardware changes need a full rebuild (rung 3).
hf spaces logs <id> --build --tail 500         # bigger window; default is small
A Space isn't done until a `gradio_client` call against the live URL exercises the endpoint end-to-end. Four steps in order; keep `hf spaces logs <id> --follow` running in another terminal throughout, so any silent fallback (model snapping to a different size, missing optional dep, dtype downgrade) surfaces.
Confirm the model finished loading, no import warnings, no "falling back to CPU" / dtype-downgrade messages, no failing health checks the platform forgave. Do this before calling the API; many silent failures (a config typo loading the wrong model, a missing optional dep, a one-time init that errored but didn't crash boot) are only visible here.
Default; sync `gr.Interface` / `gr.Blocks` / `gr.ChatInterface` / `gr.Server` `@app.api`:
print(c.view_api())                          # discover endpoints; don't guess api_name
**Streaming endpoints** (function uses `yield` or `TextIteratorStreamer`); `.predict()` returns only the final value. Iterate chunks via `.submit()`:
**OAuth-gated Spaces** (`hf_oauth: true` + `gr.LoginButton`); anonymous `Client` can't authenticate. Test interactively after sign-in, or capture a session token and pass via `httpx_kwargs={"headers": {...}}`.
**MCP server mode** (`launch(mcp_server=True)`); different protocol. Use an MCP client.
Look at the tailed run log alongside; silent fallbacks (model snapping resolution, missing optional dep falling back to a slower path, dtype downgrade) only show up there.
- **Don't launch Playwright / headless browser** to verify backend logic. The Gradio UI calls the same API `gradio_client` does; a `predict` call checks the API path; separately verify browser layout, authentication and event wiring when relevant.
- **Don't smoke-test with full-budget inputs.** Smallest input that exercises the GPU code path; short prompt, small image, low step count. You're verifying wiring, not quality.
1. Decide SDK + hardware. Write the smallest `app.py` / `Dockerfile` + `requirements.txt` + README frontmatter; just enough that the entry point loads.
1. **PRO / Team / Enterprise plan**; dev mode is a paid feature.
3. **The Space must be in `RUNNING` or `RUNTIME_ERROR`** before dev mode lets you in; not `BUILD_ERROR`. If it's in build error, push a stub `app.py` that boots cleanly first (e.g. `import gradio as gr; gr.Interface(lambda: 'ok', None, 'text').launch()`), then enable dev mode.
No `huggingface_hub` Python wrapper yet; use the REST endpoint:
**`nvidia-smi` in the bare terminal will fail** with `NVML: Unknown Error`. Expected; ZeroGPU only exposes the real GPU inside `@spaces.GPU` calls. Don't assume the GPU is broken.
Then re-run the outside-the-container smoke test. Dev-mode success does **not** guarantee post-rebuild success; different image, different process tree.