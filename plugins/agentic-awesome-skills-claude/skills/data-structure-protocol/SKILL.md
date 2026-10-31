description: "Give agents persistent structural memory of a codebase; navigate dependencies, track public APIs, and understand why connections exist without re-reading the whole repo."
LLM coding agents lose context between tasks. On large codebases they spend most of their tokens on "orientation"; figuring out where things live, what depends on what, and what is safe to change. DSP solves this by externalizing the project's structural map into a persistent, queryable graph stored in a `.dsp/` directory next to the code.
Every file or artifact that is imported anywhere must be represented in `.dsp` as an Object; code, images, styles, configs, JSON, wasm, everything. External dependencies (npm packages, stdlib, etc.) are recorded as `kind: external` but their internals are never analyzed.
- **When creating a file/module**: Call `create-object`. For each exported function; `create-function` (with `--owner`). Register exports via `create-shared`.
- **When adding an import**: Call `add-import` with a brief `why`. For external deps; first `create-object --kind external` if the entity doesn't exist.
# Shows every module that imports lodash and WHY: lets you systematically replace it
- ✅ **Do:** Always add a meaningful `why` reason when recording an import; this is where most of DSP's value lives
- ✅ **Do:** Treat `.dsp/` diffs like code diffs; review them, keep them accurate
- ❌ **Don't:** Create UIDs for every local variable or helper; only file-level Objects and public/shared entities
- **context-compression**; DSP reduces the need for compression by providing targeted retrieval instead of loading everything
- **context-optimization**; DSP is a structural optimization: agents pull minimal "context bundles" instead of raw source
- **architecture**; DSP captures architectural boundaries (imports/exports) that feed system design decisions