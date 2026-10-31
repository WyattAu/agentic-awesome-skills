Import one or more CSV, JSON, JSONL, or PDF files into a Weaviate collection with automatic type conversion and column mapping. Multiple supported tabular files can be passed in a single invocation; all objects are appended to the same collection. PDF files are converted page-by-page to base64-encoded JPEG images; the collection is created automatically on first import and reused on subsequent runs.
# CSV/JSON/JSONL: collection must already exist
# PDF: collection is created automatically on first run; appended to on subsequent runs
| `files` |; | Yes (positional, one or more) |; | One or more CSV, JSON, JSONL, or PDF files (CSV/JSON/JSONL may be mixed; PDFs imported separately) |
| `--collection` | `-c` | Yes |; | Target collection name (must already exist for CSV/JSON/JSONL; created automatically for PDF if absent, otherwise appended to) |
| `--mapping` | `-m` | No |; | JSON object mapping file columns/keys to collection properties (CSV/JSON/JSONL only) |
| `--tenant` | `-t` | No |; | Tenant name for multi-tenant collections (required if collection has multi-tenancy enabled) |
| `--skip-fields` |; | No |; | Comma-separated field names to exclude from import (e.g. `vector`) |
| `--json` |; | No | `false` | Output in JSON format |
- First row must be a header; column names must match collection property names (case-sensitive)
- The entire file is loaded into memory; for large datasets, always prefer JSONL
- Streamed line-by-line; preferred format for large datasets
- The collection is **created automatically** with `multi2vec_weaviate` (`ModernVBERT/colmodernvbert` + MUVERA encoding) if it does not already exist. If the collection already exists, pages are appended to it; allowing multiple PDFs to be loaded into the same collection across multiple runs.
| `int` / `int[]` | `int(value)`; invalid values fail |
| `number` / `number[]` | `float(value)`; invalid values fail |
| `boolean` / `boolean[]` | `"true"`/`"false"` → bool; invalid values fail |
| `text[]`, `int[]`, `number[]`, `boolean[]`, `date[]`, `uuid[]`, `object`, `object[]`, `geoCoordinates`, `phoneNumber` | JSON/JSONL: native arrays/objects must match the declared container type. CSV: cell is parsed with `json.loads()`; invalid values fail |
| `blob` | kept as string; must already be base64-encoded in the source data |