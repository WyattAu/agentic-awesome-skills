ZCode does not install a `zcode` binary on PATH and does not publish one to npm; the CLI ships
2. `zcode` on **PATH**; an actual executable/shim, not a shell alias.
   fixed install path, so there is nothing to auto-discover there; use option 1 or 2.
If no CLI is found; including when you name one explicitly that does not exist; the relay exits
| `sessionId` | The `sess_…` id; pass to `--session` to continue this exact session |
An orchestrator that polls for the file must therefore also check the exit code; a non-zero exit
  `parseWarning` field is set; read `outputPath` yourself.
- **`status: "timeout"`**; the watchdog fired. The whole ZCode process tree was killed. Inspect the
- **`status: "aborted"`**; the relay itself was killed and forwarded the kill. Same advice.
- **`status: "zcode_unavailable"`**; no CLI found. Check the three resolution routes above.
- **Exit 1 with `Session not found`**; the `--session` id does not exist. Session ids are
- **`completed` but `touchedFiles` is empty on a write run**; read `finalMessage`. ZCode may have
- **`(no final message captured)` on a resumed plan-mode run**; ZCode has been observed returning