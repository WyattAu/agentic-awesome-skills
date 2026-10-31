# Route by task complexity: don't use GPT-4o for everything
# Anthropic: cache long system prompts (saves 90% on cached tokens)
# OpenAI: prompt caching is automatic for repeated prefixes >1024 tokens
    # 3. Cache miss: call LLM
# LLMLingua: compress long prompts by 3–20× with minimal quality loss
        "Self-host now; strong ROI" if break_even < 0.5 else
        "Stick with API; not enough scale yet"
- Use `gpt-4o-mini` or `claude-haiku` for 80% of tasks; they're 10–30× cheaper.
- Audit your top 5 prompts by token count; compress or cache them.