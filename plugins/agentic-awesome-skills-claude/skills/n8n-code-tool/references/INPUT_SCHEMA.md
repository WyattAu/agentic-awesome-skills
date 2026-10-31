- Tool calls are more reliable; most modern LLMs handle structured tools better than "here's a JSON string please"
### Style A: `fromJson`: paste a representative example (v≥1.3, recommended)
### Style B: `manual`: write the JSON Schema yourself
In Python, the same applies; `_query` is a string without schema, a dict with schema.
│  └─ Unstructured; skip the schema