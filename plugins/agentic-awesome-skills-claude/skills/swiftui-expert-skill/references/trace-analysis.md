file. A target SwiftUI source file is **optional**; if provided, you can
### 2. `--list-logs`: find os_log timestamps
### 3. `--list-signposts`: find signpost intervals
### 4. `--fanin-for`: who keeps invalidating this view?
`closure #1 in UserDefaultObserver.Target.GraphAttribute.send()`; the
## Composition pattern: scoping to a slice
1. **Discover**; call `--list-logs` or `--list-signposts` with filters
2. **Build the window**; take `time_ms` (logs) or `start_ms`/`end_ms`
3. **Analyse**; call the default mode with `--window`.
   but the user-code caller one frame up is typically what to fix; say
- **`closure #1 in UserDefaultObserver.Target.GraphAttribute.send()`** -
- **`EnvironmentWriter: …`** with thousands of edges; a modifier (often
- **`View Creation / Reuse`** as the #1 source; the hierarchy is
1. **One-line summary**; "Found N hangs, worst Wms; K hitches; J high-severity SwiftUI updates."
2. **Evidence and hypotheses**; per prioritised target (see above), one paragraph with the trace evidence (coverage %, hot symbol, overlapping view) and a citation from `references/…` for the fix pattern.
3. **Plan**; numbered, file-specific edits. Cite line numbers in the user's Swift file when you know them. Don't edit the file unless the user asked for edits.