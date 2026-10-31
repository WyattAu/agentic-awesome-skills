> Portable skill; readable by Claude Code, OpenCode, Codex, Cursor, Windsurf, and others.
> This skill describes a **discipline at the network edge**; one client, one envelope, one error
> type, validated types; not a state library or a styling system. It pairs with the
JSON and becomes a **trusted, typed domain value**; or it becomes a **single, typed error**.
1. **One client is the only fetch boundary.** A single typed `apiClient` wraps `fetch`. Components and hooks never call `fetch`/`axios` directly; the boundary is enforceable in review and lint.
2. **Parse, don't validate.** Wire JSON is parsed into domain types at the boundary. After the client returns, the value is trusted everywhere downstream; no defensive `?.` chains, no re-checking shapes in components.
5. **Identifiers are branded.** Domain IDs are nominal types (`InvoiceId`, `CustomerId`) so the compiler rejects passing one where another is expected; the most common silent bug in data-heavy UIs.
// CORRECT; a feature hook wraps the client, typed by the caller
// WRONG; a raw fetch in a component bypasses the boundary entirely
- No `fetch`/`axios`/`XMLHttpRequest` outside `shared/api-client/`; enforce with an ESLint `no-restricted-imports`/`no-restricted-globals` rule.
value**; so downstream code is guaranteed correct by the type system. Run a schema parse at the
  total: z.number().int(), // minor units; never float money
> Validation library is your choice; **Zod**, **Valibot**, **ArkType**, **io-ts**. The rule is
loadInvoice(customerId); // ❌ compile error; exactly the bug you want caught
zero; brands erase at compile time.
messages (it does **not** localize; that's the UI's job).
  /** Stable key under an `errors` i18n namespace; never a raw server string. */
Server validation (`fields`) maps straight onto form-field errors; one place, typed.
- Never `throw new Error(string)` from the data layer; always `ApiError` with a `code`.
- Never show `error.message` (a server/dev string) directly to users; resolve `messageKey` through i18n.
For **React Native**, the client is unchanged; `fetch` and `AbortController` exist in RN. Only
- [ ] The client is framework-free; no toasts, router, or React inside it.
- [ ] `error.messageKey` resolves through i18n; server `message` is never shown to users.
brand its ID at parse time, and wrap `apiClient` in typed query/mutation hooks; never call the
2. Keep the frontmatter `name` and high-signal `description`; discovery indexes match against it.