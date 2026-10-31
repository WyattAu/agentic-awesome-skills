# Kubernetes NetworkPolicy: isolate LLM API
  # Block egress to internet: prevent data exfiltration
        # DO NOT log prompt/completion content: PII risk
- Never log raw prompts or completions; they may contain PII or sensitive data.
- Treat LLM output as untrusted input; always encode before rendering in HTML.