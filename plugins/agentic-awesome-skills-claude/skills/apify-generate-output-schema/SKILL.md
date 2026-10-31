- **Analyze code first**: Read the Actor's source to understand what data it actually pushes to the dataset; never guess
- **Reuse existing patterns**: Before generating schemas, check if other Actors in the same repository already have output schemas; match their structure, naming conventions, description style, and formatting
5. **Search for existing schemas in the repository**: Look for other `.actor/` directories or schema files (e.g., `**/dataset_schema.json`, `**/output_schema.json`, `**/key_value_store_schema.json`) to learn the repo's conventions; match their description style, field naming, example formatting, and overall structure
8. Find output type definitions; **reuse them directly** instead of recreating from scratch:
   - **TypeScript**: Look for output type interfaces/types (e.g., in `src/types/`, `src/types/output.ts`). If an interface or type already defines the output shape, derive the schema fields from it; do not create a parallel definition
9. Check for existing shared schema utilities or helper functions in the codebase that handle schema generation or validation; reuse them rather than creating new logic
            // ALL output fields here; every field the Actor can produce,
- Match the **field naming convention** (camelCase vs. snake_case); this must also match the actual keys produced by the Actor code
- Match the **JSON formatting** (indentation, property ordering, spacing); all schemas in the same repository must use identical formatting, including standalone Actors
| **All fields in `properties`** | The `fields.properties` object must contain **every** field the Actor can output, not just the fields shown in the overview view. The views section selects a subset for display; the `properties` section must be the complete superset |
> **Warning; most common mistakes**:
    "title": "<Descriptive title; what the key-value store contains>",
1. **Fixed keys** (e.g., `"RESULTS"`, `"summary"`); use `"key"` (exact match)
2. **Dynamic keys with a prefix** (e.g., `"screenshot-${id}"`, `f"image-{name}"`); use `"keyPrefix"`
    "title": "<Descriptive title; what the Actor returns>",
> **Critical**: Each property entry **must** include `"type": "string"`; this is an Apify-specific convention. The Apify meta-validator rejects properties without it (and rejects `"type": "object"`; only `"string"` is valid here).
- `{{links.apiDefaultDatasetUrl}}`; API URL of default dataset
- `{{links.apiDefaultKeyValueStoreUrl}}`; API URL of default key-value store
- `{{links.publicRunUrl}}`; Public run URL
- `{{links.consoleRunUrl}}`; Console run URL
- `{{links.apiRunUrl}}`; API run URL
- `{{links.containerRunUrl}}`; URL of webserver running inside the run
- `{{run.defaultDatasetId}}`; ID of the default dataset
- `{{run.defaultKeyValueStoreId}}`; ID of the default key-value store
- [ ] **Every** output field from the source code is in `dataset_schema.json` `fields.properties`; not just the overview view fields but ALL fields the Actor can produce
- [ ] Schema fields are derived from existing type definitions (interfaces, TypedDicts, dataclasses) where available; no duplicated or divergent field definitions