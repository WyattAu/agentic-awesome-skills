A common, maddening bug: an item carries both `json` and `binary`, it runs through a JSON-only node (Edit Fields, Code, IF), the binary slot quietly disappears, and the email node three steps later has nothing to attach. No error, no validation warning; just a missing file.
                          (bypass; binary passes through unchanged)
- **Transform branch:** does the JSON work; may lose binary. That's fine; this branch only contributes the JSON.
The exact parameter names (`mode`, `combineBy`, `combineByPosition`, and how `numberOfInputs` is expressed) have shifted across Merge node versions; confirm the current shape with `get_node` on `nodes-base.merge` for the user's version before committing the structure. The principle is stable; the field names move.
| `combineAll` | Cartesian product (N×M items) | ❌ No; explodes the item count |
| `append` | Concatenates inputs end to end | ❌ No; doesn't pair items |
`combineByPosition` is the right default: it keeps the item count at N and pairs each transformed JSON item with its corresponding binary-bearing original. For this to work, both branches must emit items in the same order and count; sharing a source alone does not guarantee this after filtering, sorting or retries. Prefer a stable session/item key when either can change.
If the transforming node can preserve binary itself, do that instead; it's one node, not three:
- **IF / Filter:** these route items rather than rebuild them, and generally preserve binary on the items they pass; but verify in the execution rather than assuming.
If the chain has many strip points, threading binary through all of them; and Merging at each one; becomes more work than it's worth. Two better routes:
Past a couple of strip points, one of these is usually less work; and less fragile; than keeping every node in a long chain honest about binary.