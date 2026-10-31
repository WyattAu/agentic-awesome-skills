   - `*.py`, `requirements.txt`, `pyproject.toml`, `setup.py`, `Pipfile` → **Python**; read from `python/`
   - `*.ts`, `*.tsx`, `package.json`, `tsconfig.json` → **TypeScript**; read from `typescript/`
   - `*.js`, `*.jsx` (no `.ts` files present) → **TypeScript**; JS uses the same SDK, read from `typescript/`
   - `*.java`, `pom.xml`, `build.gradle` → **Java**; read from `java/`
   - `*.kt`, `*.kts`, `build.gradle.kts` → **Java**; Kotlin uses the Java SDK, read from `java/`
   - `*.scala`, `build.sbt` → **Java**; Scala uses the Java SDK, read from `java/`
   - `*.go`, `go.mod` → **Go**; read from `go/`
   - `*.rb`, `Gemfile` → **Ruby**; read from `ruby/`
   - `*.cs`, `*.csproj` → **C#**; read from `csharp/`
   - `*.php`, `composer.json` → **PHP**; read from `php/`
| Python     | Yes (beta)  | Yes       | Full support; `@beta_tool` decorator |
| TypeScript | Yes (beta)  | Yes       | Full support; `betaZodTool` + Zod    |
> **Start simple.** Default to the simplest tier that meets your needs. Single API calls and workflows handle most use cases; only reach for agents when the task genuinely requires open-ended, model-driven exploration.
> **Note:** The Agent SDK is for when you want built-in file/web/terminal tools, permissions, and MCP out of the box. If you want to build an agent with your own tools, Claude API is the right choice; use the tool runner for automatic loop handling, or the manual loop for fine-grained control (approval gates, custom logging, conditional execution).
   └── Claude API; one request, one response
   as part of its work? (Not: does your app read a file and hand it to Claude -
   └── Yes → Agent SDK; built-in tools, don't reimplement them
   └── Claude API with tool use; you control the loop
- **Complexity**; Is the task multi-step and hard to fully specify in advance? (e.g., "turn this design doc into a PR" vs. "extract the title from this PDF")
- **Value**; Does the outcome justify higher cost and latency?
- **Viability**; Is Claude capable at this task type?
- **Cost of error**; Can errors be caught and recovered from? (tests, review, rollback)
**User-defined tools**; You define tools (via decorators, Zod schemas, or raw JSON), and the SDK's tool runner handles calling the API, executing your functions, and looping until Claude is done. For full control, you can write the loop manually.
**Server-side tools**; Anthropic-hosted tools that run on Anthropic's infrastructure. Code execution is fully server-side (declare it in `tools`, Claude runs code automatically). Computer use can be server-hosted or self-hosted.
**Structured outputs**; Constrains the Messages API response format (`output_config.format`) and/or tool parameter validation (`strict: true`). The recommended approach is `client.messages.parse()` which validates responses against your schema automatically. Note: the old `output_format` parameter is deprecated; use `output_config: {format: {...}}` on `messages.create()`.
**Supporting endpoints**; Batches (`POST /v1/messages/batches`), Files (`POST /v1/files`), and Token Counting feed into or support Messages API requests.
→ Read `{lang}/claude-api/README.md`; see Compaction section
1. **`{language}/claude-api/README.md`**; **Read this first.** Installation, quick start, common patterns, error handling.
2. **`shared/tool-use-concepts.md`**; Read when the user needs function calling, code execution, memory, or structured outputs. Covers conceptual foundations.
3. **`{language}/claude-api/tool-use.md`**; Read for language-specific tool use code examples (tool runner, manual loop, code execution, memory, structured outputs).
4. **`{language}/claude-api/streaming.md`**; Read when building chat UIs or interfaces that display responses incrementally.
5. **`{language}/claude-api/batches.md`**; Read when processing many requests offline (not latency-sensitive). Runs asynchronously at 50% cost.
6. **`{language}/claude-api/files-api.md`**; Read when sending the same file across multiple requests without re-uploading.
7. **`shared/error-codes.md`**; Read when debugging HTTP errors or implementing error handling.
8. **`shared/live-sources.md`**; WebFetch URLs for fetching the latest official documentation.
> **Note:** For Java, Go, Ruby, C#, PHP, and cURL; these have a single file each covering all basics. Read that file plus `shared/tool-use-concepts.md` and `shared/error-codes.md` as needed.
1. **`{language}/agent-sdk/README.md`**; Installation, quick start, built-in tools, permissions, MCP, hooks.
2. **`{language}/agent-sdk/patterns.md`**; Custom tools, hooks, subagents, MCP integration, session resumption.
3. **`shared/live-sources.md`**; WebFetch URLs for current Agent SDK docs.