description: "Expert in Drizzle ORM for TypeScript; schema design, relational queries, migrations, and serverless database integration. Use when building type-safe database layers with Drizzle."
// Infer types directly from your schema; no separate type files needed
// Nested includes; Drizzle resolves in a single query
# Push schema directly to database (development only: skips migration files)
- ❌ **Don't:** Use `drizzle-kit push` in production; it can cause data loss
- ❌ **Don't:** Create a new database connection per request in serverless; use connection pooling