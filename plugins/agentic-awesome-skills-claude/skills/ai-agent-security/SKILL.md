## Threat Model: STRIDE for AI Agents
**Prompt Injection**; Untrusted content (user input, web scrapes, document contents) manipulates the agent's system prompt or reasoning chain to execute unintended actions.
**Tool Abuse**; The agent calls tools in sequences or with parameters the designer did not anticipate, achieving effects beyond its intended scope.
**Data Exfiltration**; The agent encodes sensitive data (credentials, PII, internal IPs) into its responses, tool calls, or outbound HTTP requests.
**Cross-Tenant Leakage**; In multi-tenant deployments, context from one tenant's session bleeds into another through shared memory, vector stores, or cache.
**Privilege Escalation**; The agent chains low-privilege tool calls to achieve high-privilege outcomes (e.g., read config -> extract credentials -> call admin API).