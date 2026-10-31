1. Trust boundaries; where does untrusted input enter the system? (HTTP requests, CLI args, file reads, IPC, message queues, environment variables, config files, etc.)
2. Authentication; how do callers prove identity? (sessions, tokens, API keys, mTLS, Unix sockets, etc.) If there's no authentication, note that.
3. Authorization; how are permissions enforced? (middleware, decorators, capability checks, file permissions, etc.) If there's no authorization model, note that.
4. Privilege separation; does the code run as root? Drop privileges? Use sandboxing? Fork workers?
1. Network-facing surfaces (HTTP endpoints, gRPC services, WebSocket handlers, TCP/UDP listeners, etc.); list each with method/verb and purpose
- Include the key file paths from all agents; these become the starting points for Phase 2