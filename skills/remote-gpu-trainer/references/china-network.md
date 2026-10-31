Universal recipe for pulling code, packages, and model weights onto **any GPU box behind the GFW** -
Universal gotchas (inode caps, silent sync, symlinked caches) are **not** restated here; see
1. Mirrors table; PyPI / conda / HuggingFace / alt hub
2. Env switchboard; the four switches + the import-time trap + cache redirect
3. Resumable-download ladder; three tiers + the `hf_transfer` caution
4. The `no_proxy` trap; a proxy that fixes one domain breaks all the others
Swap the *source*, not the workflow. Same package names, same repo IDs; only the endpoint changes. Ship
| **HuggingFace** | `export HF_ENDPOINT=https://hf-mirror.com` | drop-in reverse proxy; identical repo IDs, identical `hf download` / `from_pretrained` calls |
| **Alt model hub** | ModelScope CLI / SDK | `pip install modelscope`; `modelscope download <id>` or `snapshot_download(id, ...)`; often hosts the same Qwen / GLM / Llama weights domestically |
**conda trap; NEVER mirror `pytorch-nightly`.** TUNA (and every CN Anaconda mirror) syncs the stable
`pytorch` channel but **does not carry `pytorch-nightly`**; pointing the nightly channel at a mirror
Everything below is **environment variables only**; no code edits. Export them once per shell (or bake
**The import-time trap; `HF_ENDPOINT` is read once, at import.** `huggingface_hub` / `transformers` /
notebook cell run after the first `import transformers`) is a no-op; the library already cached the
# Inline on the command: the env is set before the interpreter starts:
**Cache redirect; why it matters.** Most CN images pair a tiny reset-on-release system disk with a larger
Bulk weight pulls are the prototypically flaky step on a CN link; a stall is **not** a permanent failure,
**Tier 1; `hf download <repo> ` (default).**
(Underlying verbs; `hf download `, `hf cache verify`; belong to **REQUIRED:**
**Tier 2; `hfd.sh` (aria2 multi-connection) for any single file > 10 GB.**
`hfd.sh` (the HF-Mirror companion script) drives `aria2c` with many parallel connections per file -
**Tier 3; ModelScope `snapshot_download` (HTTP-Range resume).**
When a model exists on ModelScope (most CN-origin models do), pull it domestically; `snapshot_download`
`from_pretrained("Org/Model")` won't find it; point the load at the local dir.
**`hf_transfer` caution; keep `HF_HUB_ENABLE_HF_TRANSFER=0` on flaky CN networks.**
hang-with-no-error** in exactly the unstable-bandwidth conditions CN ops hit; the download wedges with no
**simultaneously breaks every domestic mirror**; `pip`, the TUNA index, ModelScope, intra-cloud OSS all
- **Leading-dot domains, no `*` wildcards.** `requests` honors `no_proxy` but does **not** expand `*`; use
- **Proxy handling depends on pip/requests configuration**; pass `pip install --proxy ""` to force pip onto the
set. On a freshly rented box with no `http_proxy`/`https_proxy` exported, adding `no_proxy` does nothing -
1. **hf-mirror first**; `HF_ENDPOINT=https://hf-mirror.com`. Drop-in, same repo IDs, no proxy, no
2. **ModelScope** if the model is absent on the mirror or the mirror route is flaky; same Qwen / GLM /
3. **`hfd.sh`** for any single file > 10 GB on a stable-but-slow link; aria2 multi-connection.
   domestic mirrors keep working; and unset both when the pull is done.
**Ship `scripts/setup-china-mirrors.sh`**; the orchestrator `scp`s it onto the box and `source`s it on