description: "Agent governance skill for MCP tool calls; Cedar policy authoring, shadow-to-enforce rollout, and Ed25519 receipt verification."
1. **Shadow mode** (default); logs decisions without blocking. Use this to observe what your policies would do before enforcing them.
2. **Enforce mode**; blocks tool calls that violate policy. Use after shadow-mode validation.
3. **Hooks mode**; integrates with Claude Code hooks for pre/post tool-call governance.
Cedar is a policy language designed for authorization. Policies are evaluated locally via WASM; no network calls required.
// Start permissive; allow everything in shadow mode
# Shadow mode is the default: logs decisions without blocking
- ❌ **Don't:** Treat a valid signature as proof the signer is trustworthy; it only proves the receipt has not been tampered with since signing
- `@security-auditor`; General security auditing and compliance
- `@security-audit`; Code vulnerability scanning
- `@mcp-development`; MCP server development patterns
- [protect-mcp on npm](https://www.npmjs.com/package/protect-mcp); MIT licensed
- [Cedar Policy Language](https://www.cedarpolicy.com/); AWS open-source policy engine
- [IETF Draft: Signed Receipts](https://datatracker.ietf.org/doc/draft-farley-acta-signed-receipts/); Receipt format specification
- [@veritasacta/verify](https://www.npmjs.com/package/@veritasacta/verify); Apache-2.0 verifier, works offline