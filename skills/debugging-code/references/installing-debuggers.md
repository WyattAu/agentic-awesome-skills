`dap` relies on language-specific debug adapters (backends). Many IDEs already ship them; **check before installing**.
## Python: debugpy
to `python3` on PATH; which may not have `debugpy` installed or may be the wrong
## Go: Delve
## Node.js/TypeScript: js-debug
**Check:** Look for js-debug in the paths above, or run `dap debug --backend js-debug script.js`; if it fails, install below.
## Rust/C/C++: lldb-dap