A unified API gateway that routes LLM requests across providers and self-hosted models; with rate limiting, cost tracking, caching, and failover.
## LiteLLM Proxy: Quick Start
      api_base: http://vllm-2:8000/v1  # second replica; auto load balanced
# nginx.conf: round-robin across vLLM replicas
- Use virtual keys per team/app; never expose raw provider API keys.