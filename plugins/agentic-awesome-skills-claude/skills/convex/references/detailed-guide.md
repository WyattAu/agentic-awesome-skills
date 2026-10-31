- **TypeScript-first**: All backend logic; queries, mutations, actions, schemas; is written in TypeScript
Queries are reactive; clients automatically get updates when data changes.
// Simple query; list all tasks
Actions can call third-party services but cannot directly access the database; they must use `ctx.runQuery` and `ctx.runMutation`.
// app/layout.tsx; wrap children
  // Reactive query; auto-updates when data changes
Better Auth provides email/password, social logins, two-factor authentication, and session management; all running inside Convex functions rather than an external auth server.
  // Reactive signal; updates automatically when data changes
- ✅ Define schemas; adds type safety across your entire stack
- ✅ Use indexes for queries; avoids full table scans
- ✅ Rely on native determinism; `Date.now()` and `Math.random()` are handled deterministically by the runtime, but query time does not itself create a timer subscription; do not use Math.random for secrets!
- ✅ Use `ctx.runQuery` / `ctx.runMutation` from actions; never access `ctx.db` directly in actions
- ✅ Add argument validators to all functions; they enforce runtime type safety
  **Solution:** This is expected; Convex queries are async. Check for `undefined` before rendering (this means loading, not empty).
- `@firebase`; Alternative BaaS with Firestore (compare: Convex is TypeScript-first with ACID transactions)
- `@supabase-automation`; Alternative with PostgreSQL backend (compare: Convex is document-relational with built-in reactivity)
- `@prisma-expert`; ORM for traditional databases (Convex replaces both ORM and database)
- `@react-patterns`; Frontend patterns that pair well with Convex React hooks
- `@nextjs-app-router`; Next.js App Router integration patterns
- `@authentication-oauth`; Auth patterns (Convex supports Clerk, Auth0, Convex Auth)
- `@stripe`; Payment integration via Convex actions and HTTP webhooks