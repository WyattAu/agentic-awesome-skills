# zeroize-audit: Claude Skill
| `path` | yes |; | Repo root |
| `config` | no |; | YAML defining heuristics and approved wipes |
| `max_tus` | no |; | Limit on translation units processed from compile DB |
| `mcp_mode` | no | `prefer` | `off`, `prefer`, or `require`; controls Serena MCP usage |
| `mcp_timeout_ms` | no |; | Timeout budget for MCP semantic queries |
| `compile_commands.json` at `compile_db` path | Fail fast; do not proceed |
| `clang` on PATH | Fail fast; IR/ASM analysis impossible |
| `{baseDir}/tools/extract_compile_flags.py` | Fail fast; cannot extract per-TU flags |
| `{baseDir}/tools/emit_ir.sh` | Fail fast; IR analysis impossible |
| `Cargo.toml` at `cargo_manifest` path | Fail fast; do not proceed |
| `cargo check` passes | Fail fast; crate must be buildable |
| `cargo +nightly` on PATH | Fail fast; nightly required for MIR and LLVM IR emission |
| `uv` on PATH | Fail fast; required to run Python analysis scripts |
| `{baseDir}/tools/validate_rust_toolchain.sh` | Warn; run preflight manually. Checks all tools, scripts, nightly, and optionally `cargo check`. Use `--json` for machine-readable output, `--manifest` to also validate the crate builds. |
| `{baseDir}/tools/emit_rust_mir.sh` | Fail fast; MIR analysis impossible (`--opt`, `--crate`, `--bin/--lib` supported; `--out` can be file or directory) |
| `{baseDir}/tools/emit_rust_ir.sh` | Fail fast; LLVM IR analysis impossible (`--opt` required; `--crate`, `--bin/--lib` supported; `--out` must be `.ll`) |
| `{baseDir}/tools/scripts/check_rust_asm.py` | Warn and skip Rust assembly analysis (`STACK_RETENTION`, `REGISTER_SPILL`, drop-glue checks). Dispatches to `check_rust_asm_x86.py` (production) or `check_rust_asm_aarch64.py` (**EXPERIMENTAL**; AArch64 findings require manual verification). |
| `{baseDir}/tools/generate_poc.py` | Fail fast; PoC generation is mandatory |
Phase 0: 0-preflight agent; Preflight + config + create workdir + enumerate TUs
Phase 8: Orchestrator; Return final-report.md
1. **`final-report.md`**; Comprehensive markdown report (primary human-readable output)
2. **`findings.json`**; Structured JSON matching `{baseDir}/schemas/output.json` (for machine consumption and downstream tools)
| Exit 0 (exploitable) | Yes | Strong signal; can upgrade `likely` to `confirmed` |
| Exit 0 or 1 | No (user accepted) | Weaker signal; note verification failure in evidence |
| Compile failure / no PoC |; | No confidence change; annotate in evidence |
- *"The compiler won't optimize this away"*; Always verify with IR/ASM evidence. Never suppress `OPTIMIZED_AWAY_ZEROIZE` without it.
- *"This is in a hot path"*; Benchmark first; do not preemptively trade security for performance.
- *"Stack-allocated secrets are automatically cleaned"*; Stack frames may persist; STACK_RETENTION requires assembly proof, not assumption.
- *"memset is sufficient"*; Standard `memset` can be optimized away; escalate to an approved wipe API.
- *"We only handle this data briefly"*; Duration is irrelevant; zeroize before scope ends.
- *"This isn't a real secret"*; If it matches detection heuristics, audit it. Treat as sensitive until explicitly excluded via config.
- *"We'll fix it later"*; Emit the finding; do not defer or suppress.