# Test Guard: JavaScript / TypeScript / Jest / Vitest Patterns
- Network: prefer `msw` (Mock Service Worker) over `jest.mock`-ing your own fetch wrapper; it mocks at the true boundary
- `jest.mock('../utils/helpers')`; mocking your own internal module to isolate a "unit" (Rule 2)
- Full component trees that change on every styling tweak (Rule 1; brittle, asserts implementation)
- Large objects nobody reviews; an unread snapshot approves itself (Rule 4)
For data-layer logic (Prisma/Drizzle/Knex queries), run against a real test database; `testcontainers`, a Dockerized Postgres, or SQLite where compatible. Mocking the query builder to test the query builder tests nothing.