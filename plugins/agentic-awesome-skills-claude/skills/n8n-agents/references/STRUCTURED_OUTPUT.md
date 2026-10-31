- **Required vs optional fields**; an example is one snapshot; the parser can't tell which keys are mandatory.
- **Enums**; `"category": "compliance"` doesn't constrain the model to `compliance | history | risk`; it will invent new categories.
- **Numeric ranges**; `"score": 3` doesn't say `1-5`; the model returns `7` or `0.85` and passes.
- **Array constraints**; min/max items, item-type uniformity.
- **String formats**; email, UUID, ISO date, regex.
Reach for `fromJson` + `jsonSchemaExample` only for one-off shapes you're certain will never grow constraints. Once a field needs to be optional, enum-ed, or range-bounded, you're rewriting the parser anyway; start with the schema.
The fixer is wired as a **separate** sub-node into the parser's `ai_languageModel` slot. **Use a coding-capable model** (Sonnet-class or better). Reconciling broken JSON against a schema with enums, ranges, and required fields is a structured-output / coding task; a weak or generic model routinely produces another malformed retry, defeating the point and burning tokens.
This line is **load-bearing**. Models default to wrapping JSON in triple-backtick `json` fences, which breaks the parser. If you see parse failures on output that's clearly valid JSON inside a code block, this instruction is the fix; in both the retry prompt and, if the main model wraps aggressively, the **main** system prompt:
- **Free-form chat replies to the user**; conversational text doesn't need parsing.
- **Tool calls only, no final structured output**; if the user-visible output is text, skip it.
- **Trivial key-value extraction**; JSON.parse alone checks syntax, not schema or business meaning; validate the parsed object and fail closed downstream.