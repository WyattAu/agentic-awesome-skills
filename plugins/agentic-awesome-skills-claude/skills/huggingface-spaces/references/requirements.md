The Gradio SDK base image already installs these on every hardware tier; listing them in `requirements.txt` causes resolution failures or, worse, lets pip silently drift the runtime out of compatibility:
| `torch` | **Pin only a currently supported version.** Unsupported versions can cause `CONFIG_ERROR: torch version in requirements.txt is not compatible`. Default is to leave unpinned (runtime preinstalls 2.11), but pinning is appropriate when (a) a specific version is known-good for your model, (b) you're matching a CUDA-extension wheel's `torch2.X` tag, or (c) a dep would otherwise drag torch outside the supported set. When you pin torch, also pin `torchvision` / `torchaudio` to the matching minor; see the "Torch-family side-car drift" section below. |
- `torchvision`, `torchaudio`; **not** preinstalled. Leave unpinned; pip resolves against the installed `torch` major.minor.
- `accelerate`; needed whenever you use `device_map=`. Listing it also silences `low_cpu_mem_usage=False` warnings.
- `sentencepiece`; required by most LLM tokenizers; rarely transitive.
- `einops`; required by `flash_attn.layers.rotary` and many model repos.
If a research repo ships a Python package directory (`models/`, `pipeline/`, …), just upload the directory with the rest of the Space; the whole repo root is importable as `/home/user/app`. **Do not** try to reference local paths from `requirements.txt`.
The supported ZeroGPU torch list changes; consult the official compatibility table before pinning. Default is unpinned (runtime preinstalls the latest). Pinning is fine; and sometimes warranted; within that set:
- A direct-URL CUDA wheel encodes a `torch2.X` tag (see "Prebuilt CUDA wheels" below); pin torch to match.
`2.8.0` is the safest fallback for old requirements that refuse modern torch. `2.10.0` / `2.11.0` is the sweet spot for new code. When you pin torch, also pin `torchvision` / `torchaudio` to the matching minor; see the side-car drift section.
## Prebuilt CUDA wheels: the Blackwell wheels dataset
- `pt212-cu130-cp310`; built against torch 2.12 / CUDA 13.0 / Python 3.10. **Reported by the upstream snapshot on torch 2.11; not a general compatibility guarantee.**
- `pt212-cu130-cp312`, `pt212-cu130-cp313`; same matrix at other Python versions.
- `pt28-cu128-cp310`; older fallback for Spaces stuck on torch 2.8 / CUDA 12.8.
| `flash_attn` | `flash_attn-2.8.3-cp310-cp310-linux_x86_64.whl` | Committed `flash_attn/` stub package; `sys.modules["flash_attn"] = ...` injection | **cp310 only**; requires `python_version: "3.10"` in README. Needs `einops` for `flash_attn.layers.rotary`. Real `flash_attn_2_cuda` satisfies xformers' `hasattr(flash_attn.flash_attn_interface, "flash_attn_gpu")` probe. |
| `nvdiffrast` | `nvdiffrast-0.4.0-cp310-cp310-linux_x86_64.whl` | Runtime build with `TORCH_CUDA_ARCH_LIST=12.0` | Needs `numpy`. `RasterizeGLContext` in 0.4.0 is a deprecation alias for `RasterizeCudaContext`; no headless-GL footgun. |
# README frontmatter: pin Python to match wheel cell
**Do not** install these from `@spaces.GPU` startup. The previous "subprocess.check_call pip install at first GPU acquire" pattern is now strictly worse than the wheel URL; slower cold start, eats `duration` budget, breaks reproducibility, and the build sometimes exceeds the `@spaces.GPU(duration=1500)` cap.
1. **kernels-community**; https://huggingface.co/kernels-community handles ABI matching for you. Often the simplest path; no version pinning needed.
2. **Upstream wheel matrix**; e.g. flash-attention's releases page ships a fairly complete `cu12 / torch / Python` matrix at https://github.com/Dao-AILab/flash-attention/releases. Pin `torch==X.Y.Z` in `requirements.txt` to match the wheel's `torch2.X` tag.
3. **Build it yourself and host on HF Hub.** Last resort; see [`debugging.md`](debugging.md) for the in-`@spaces.GPU` source-build pattern as a stopgap while a wheel is being built.