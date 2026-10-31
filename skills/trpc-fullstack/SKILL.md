description: "Build end-to-end type-safe APIs with tRPC; routers, procedures, middleware, subscriptions, and Next.js/React integration patterns."
tRPC lets you build fully type-safe APIs without writing a schema or code-generation step. Your TypeScript types flow from the server router directly to the client; so every API call is autocompleted, validated at compile time, and refactoring-safe. Use this skill when building TypeScript monorepos, Next.js apps, or any project where the server and client share a codebase.
A **router** groups related **procedures** (think: endpoints). Procedures are typed functions; `query` for reads, `mutation` for writes, `subscription` for real-time streams.
All procedure inputs are validated with Zod schemas. The validated, typed input is available in the procedure handler; no manual parsing.
`context` is shared state passed to every procedure; auth session, database client, request headers, etc. It is built once per request in a context factory. **Important:** Next.js App Router and Pages Router require separate context factories because App Router handlers receive a fetch `Request`, not a Node.js `NextApiRequest`.
 * `opts.req` is the fetch Request; auth is resolved server-side via `auth()`.
  const session = await auth(); // server-side auth; no req/res needed
// Export the type for the client; never import the appRouter itself on the client
    // opts is FetchCreateContextFnOptions; req is the fetch Request
Use `createServerContext`; the dedicated server-side factory; so that `auth()` is called
  // Uses createServerContext; calls auth() server-side, no req/res cast needed
// Client usage; requires wsLink in the client config
- ✅ **Export only `AppRouter` type** from server code; never import `appRouter` on the client
- ✅ **Use separate context factories**; `createTRPCContext` for the HTTP handler, `createServerContext` for Server Components and callers
- ✅ **Validate all inputs with Zod**; never trust raw `input` without a schema
- ❌ **Don't cast context with `as any`** to silence type errors; the mismatch will surface as a runtime failure when auth or session lookups return undefined
- ❌ **Don't use `createContext({} as any)`** in Server Components; use `createServerContext()` which calls `auth()` directly
- ❌ **Don't put business logic in the route handler**; keep it in the procedure or a service layer
- ❌ **Don't share the tRPC client instance globally**; create it per-provider to avoid stale closures
- Always enforce authorization in `protectedProcedure`; never rely on client-side checks alone
- Avoid exposing internal error details to clients; use `TRPCError` with a public-safe `message` and keep stack traces server-side only
- `@typescript-expert`; Deep TypeScript patterns used inside tRPC routers and generic utilities
- `@react-patterns`; React hooks patterns that pair with `trpc.*.useQuery` and `useMutation`
- `@test-driven-development`; Write procedure unit tests using `createCallerFactory` without an HTTP server
- `@security-auditor`; Review tRPC middleware chains for auth bypass and input validation gaps
- [create-t3-app](https://create.t3.gg); Production Next.js starter with tRPC wired in