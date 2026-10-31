1. **Begin Analysis**; Start the analysis operation with `begin_analyze()` (returns a poller)
2. **Poll for Results**; Poll until analysis completes (SDK handles this with `.result()`)
3. **Process Results**; Extract structured results from `AnalyzeResult.contents`
1. **Use `begin_analyze` with `AnalyzeInput`**; this is the correct method signature
2. **Access results via `result.contents[0]`**; results are returned as a list
6. **Handle long-running operations**; video/audio analysis can take minutes