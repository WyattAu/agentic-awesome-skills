### Step 1: Gather Context
| Authentication type(s)? | `securitySchemes`; see reference |
If the user provides code (Express routes, FastAPI, Django URLs, Spring controllers, etc.), **extract endpoints automatically**; do not ask what the user already told you.
### Step 2: Build the Spec
Follow the structure guide for the chosen version. Always produce a **complete, valid spec**; never leave placeholder comments like `# TODO: add schema`.
### Step 3: Schemas and Models
### Step 4: Security Schemes
| API Key (query) | `apiKey`, in `query` | Avoid; leaks in logs |
### Step 5: Parameters
**Path parameters**; always `required: true`:
**Query parameters**; document defaults and enums:
**Headers**; include `X-Request-ID`, correlation IDs, etc. as common parameters defined under `components/parameters`.
### Step 6: Response Codes
### Step 7: Quality Checklist
### Step 8: Output
- `reference/security-schemes.md`; Detailed security scheme examples for all auth types
- `reference/common-patterns.md`; Pagination, HATEOAS, problem+json, webhooks, file upload patterns