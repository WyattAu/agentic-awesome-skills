| `name` |; | Yes (positional) |; | Collection name (must be the exact approved name beginning with uppercase) |
| `--properties` | `-p` | Yes |; | JSON array of property definitions |
| `--description` | `-d` | No |; | Collection description; **strongly recommended**. Weaviate agents (Query Agent, Personalization Agent) read this to understand what the collection contains and decide which collection to query |
| `--replication-factor` | `-r` | No |; | Replication factor (defers to server default when not set) |
| `--json` |; | No | `false` | Output in JSON format |
- `description` (optional): Human-readable description; **strongly recommended**. The Query Agent reads property descriptions to understand your schema, choose the right collection, and construct accurate queries. Good descriptions include units, formats, and valid values (e.g., `"Price in US dollars (USD)"`, `"ISO two-character country code"`, `"Date the paper was published on arXiv"`)
- `tokenization` (optional): For text types; `word`, `lowercase`, `whitespace`, or `field`
- `index_range_filters` (optional): Enable range-comparison index (`>`, `<`, `>=`, `<=`, `between`) for supported scalar numeric/date types (check the pinned server; array support is not implied). Default `false`; **set to `true` for any numeric or date field you plan to range-filter**
- `nested_properties` (optional): For `object` / `object[]` types; array of nested property definitions
Before creating a collection, inspect a few rows from the source file to understand field names and value types. Use the commands below; CSV and JSONL examples below read only three records; JSON parses the entire file and must be bounded before loading.
**Important:** `id`, `_id`, and `_additional` are reserved by Weaviate; never use them as property names. If they appear in your data, use `--skip-fields` or `--mapping` in `import.py` to handle them.