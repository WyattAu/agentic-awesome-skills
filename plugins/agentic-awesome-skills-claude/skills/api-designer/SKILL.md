  "field": "type; description",
  "field2": "type; description"
**Success Response**; `STATUS_CODE Description`
| `400` | Bad Request; invalid or missing fields |
| `401` | Unauthorized; missing or invalid token |
| `403` | Forbidden; insufficient permissions |
| `409` | Conflict; e.g. duplicate resource |
| `422` | Unprocessable Entity; validation failed |
7. **Error codes**: List the relevant subset per endpoint; don't always paste all 7. Use judgement.
- Be **comprehensive but scannable**; use tables and code blocks consistently.