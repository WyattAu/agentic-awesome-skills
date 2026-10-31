# Anthropic: cache long system prompts (saves 90% on cached input tokens)
# Long system prompt: mark for caching
# OpenAI: automatic for repeated prefixes (≥1,024 tokens)
- Start with exact cache; zero cost, instant wins for identical queries.
- Always filter by model name in semantic cache; different models give different answers.