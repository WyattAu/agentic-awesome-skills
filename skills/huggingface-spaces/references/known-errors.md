Check if this is a known issue before trying your own fix. Entries are keyed by the substring that actually appears in `runtime.errorMessage`, the build log, or a Python traceback; grep this file for the error you saw.
These come from the Space build pipeline before the app starts. Read with `hf spaces logs <id> --build --tail 500`; find the **first** error, not the last.
**Fix**: Unpin torch (preferred; the runtime preinstalls the latest supported version), or pin to one of the supported values.
**Fix**: Bump or unpin the offender. Typical culprits: `deepspeed==0.15.x` (training-only; usually safe to drop from inference Spaces), `openai-whisper==20231117`.
**Cause**: README frontmatter failed server validation. Most common: `short_description` over the (undocumented) character cap; target ≤ 60.
**Cause**: The visitor's remaining quota < `requested duration`. The comparison is `requested vs remaining`, not `actual vs remaining`; a 10-second task left at the default 60 s blocks the user as soon as their remaining drops below 60 s.
**Cause**: An argument crossing the fork boundary contains an unpicklable object; file handle, lock, lambda, closure, or `gr.SelectData`.
**Cause**: Same as above; `gr.SelectData` doesn't survive pickle.
**Fix**: Same; extract its fields before crossing the boundary.
**Cause**: A Flash Attention 3 kernel was loaded; directly via `kernels-community/{flash-attn3,vllm-flash-attn3,sgl-flash-attn3}`, or indirectly via an old `xformers` wheel that auto-dispatched to FA3. FA3 has no Blackwell sm_120 build.
**Fix**: Use `attn_implementation="sdpa"`, or `"flash_attention_2"` with the FA2 wheel from `multimodalart/zerogpu-blackwell-wheels`. For xformers, the prebuilt wheel from the same dataset auto-dispatches to FA2; no monkey-patch.
**Fix**: Same; SDPA or FA2.
**Cause**: A direct-URL prebuilt CUDA wheel pinned to an old torch ABI (`cu12torch2.4cxx11abiFALSE`, etc.); won't load on the current runtime.
**Fix**: For streaming, use `client.submit(...).result()` (or iterate the job). For custom routes, use `httpx.post(base_url + "/route", ...)` directly; bypass `gradio_client`.