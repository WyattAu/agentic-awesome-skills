description: "Build production-ready AI agents with PydanticAI; type-safe tool use, structured outputs, dependency injection, and multi-model support."
# PydanticAI: Typed AI Agents in Python
PydanticAI is a Python agent framework from the Pydantic team that brings the same type-safety and validation guarantees as Pydantic to LLM-based applications. It supports structured outputs (validated with Pydantic models), dependency injection for testability, streamed responses, multi-turn conversations, and tool use; across OpenAI, Anthropic, Google Gemini, Groq, Mistral, and Ollama. Use this skill when building production AI agents, chatbots, or LLM pipelines where correctness and testability matter.
# Simple agent: returns a plain string
Register tools with `@agent.tool`; the LLM can call them during a run:
# Second turn: passes conversation history
- ✅ Always define `output_type` with a Pydantic model; avoid returning raw strings in production
- ✅ Use `deps_type` with a dataclass for dependency injection; makes agents testable
- ✅ Use `TestModel` in unit tests; never hit a real LLM in CI
- ❌ Don't put secrets (API keys) in `Agent()` arguments; use environment variables
- ❌ Don't share a single `Agent` instance across async tasks if deps differ; create per-request instances or use `agent.run()` with per-call `deps`
- ❌ Don't catch `ValidationError` broadly; let PydanticAI retry with `ModelRetry` for recoverable LLM output errors
- Set API keys via environment variables (`OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, etc.); never hardcode them.
- Validate all tool inputs before passing to external systems; use Pydantic models or manual checks.
- **Problem:** `ValidationError` on every LLM response; structured output never validates
  **Solution:** Write a clear, specific docstring for the tool function; PydanticAI sends the docstring as the tool description to the LLM.
  **Solution:** Use `await agent.run()` directly in async FastAPI route handlers; don't wrap in `asyncio.run()`.
- `@langchain-architecture`; Alternative Python AI framework (more flexible, less type-safe)
- `@llm-application-dev-ai-assistant`; General LLM application development patterns
- `@fastapi-templates`; Serving PydanticAI agents via FastAPI endpoints
- `@agent-orchestration-multi-agent-optimize`; Orchestrating multiple PydanticAI agents