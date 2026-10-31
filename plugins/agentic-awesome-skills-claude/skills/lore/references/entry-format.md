| `#stale:YYYY-MM-DD` | Flagged by `sync` as no longer accurate. Two cases: (a) the entry was superseded; pair with `#superseded-by:<new-id>`; (b) deprecated with no successor; alone. |
Syntax: `#superseded-by:LAYER-YYYY-MM-DD-xxxx`; the replacement entry's full ID. The replacement entry itself carries no back-reference; its `#verified:DATE` and `#added:DATE` are sufficient.
Worked example; bcrypt replaces SHA-256 in `scopes/backend/DECISIONS.md`:
- `find_stale.py --json`; groups stale entries by their `replaced_by` target; flags chains where the target ID does not exist (broken chain).
- `history.py --follow-superseded <id>`; prints the entry plus every successor along the chain (starting entry first, then successive replacements).
- `compress`; skips entries with `replaced_by` set when selecting the 3–5 entries per (scope, layer).
- `audit`; when reporting CONFLICT between two entries, surfaces the chain if both belong to one.