> This digest highlights key entries; open the referenced files under `_global/` and `scopes/` for the full text.
- <bullet>; [_global/ARCHITECTURE.md#<ID>]
3. Tiebreaker: entries that contain "primary" / "main" / "core" / "use <X>"; these are typically the anchor facts
> This digest highlights key entries; open the referenced files under `_global/` and `scopes/` for the full text.
- Monorepo with pnpm workspaces + Turborepo; [_global/ARCHITECTURE.md#ARCH-2026-01-15-d7a3]
- Node.js 20 baseline; [_global/ARCHITECTURE.md#ARCH-2026-02-01-9b1c]
- Rejected Nx → chose Turborepo (faster builds, simpler config); [_global/DECISIONS.md#DEC-2026-02-03-7c19]
- All packages use TypeScript strict mode; [_global/CONVENTIONS.md#CONV-2026-01-20-b1e8]
- Next.js 14 App Router; [scopes/frontend/ARCHITECTURE.md#ARCH-2026-03-10-a1b2]
- TanStack Query for server state; [scopes/frontend/ARCHITECTURE.md#ARCH-2026-03-15-e5f6]
- Zustand over Redux (60% less boilerplate); [scopes/frontend/DECISIONS.md#DEC-2026-02-03-7c19]
- No default exports; [scopes/frontend/CONVENTIONS.md#CONV-2026-04-12-c3d4]
- Node.js + Fastify + PostgreSQL; [scopes/backend/ARCHITECTURE.md#ARCH-2026-01-15-e5f6]
- Fastify over Express (3x throughput in our benchmarks); [scopes/backend/DECISIONS.md#DEC-2026-02-10-a8c9]
- All DB queries go through repository pattern; [scopes/backend/CONVENTIONS.md#CONV-2026-03-01-b1d2]
Running `compress` twice without intervening `sync`s produces identical content (modulo the `Last compressed:` date). This is intentional; compress is a pure projection of the underlying entries.