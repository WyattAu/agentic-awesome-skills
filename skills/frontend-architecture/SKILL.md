> Portable skill; readable by Claude Code, OpenCode, Codex, Cursor, Windsurf, and others.
The goal: a codebase where any contributor can instantly answer three questions -
**"where does this code live?"**, **"what is allowed to import what?"**, and **"is this server state or UI state?"** -
Everything below is the mechanical application of these five ideas. None of it is tied to a specific library; pick your stack in Sections 4 and 6.
├── app/ or routes/ or navigation/   ← framework routing layer (thin; see §7)
│       ├── index.ts                 ← PUBLIC BARREL; the only cross-module entry point
│       ├── stores/                  ← UI/client state store(s); never server data
    ├── store/                       ← root store wiring (if your state lib needs one; see §4)
// CORRECT; consume the public surface
// WRONG; reaching into internals couples you to private structure
Keep the barrel curated. If something isn't exported, it's private by design. Group exports with short comments (pages, hooks, store, types); future readers use the barrel as the module's API docs.
A page is a route the router mounts (a "screen" in React Native). It is **always a folder**, never a loose file; even when it starts as a single component. This keeps growth in place: when the page needs a sub-component or a hook, there is already a home for it.
├── {page}.styles.ts    ← every style for this page (no inline styles; see §5)
| **Server state**      | fetched entities, lists, aggregates; anything the API owns                       | a **query/cache layer** (e.g. TanStack Query, RTK Query, SWR, Apollo)                   |
- **Never drive continuous values through re-render state.** Scroll progress, pointer position, drag offset; use refs / animation values, not render state (it re-renders the tree every frame).
### 4.2 Choosing a client-state library: same shape, different syntax
**Zustand**; `modules/{feature}/stores/{feature}.store.ts`
**Redux Toolkit**; `modules/{feature}/stores/{feature}.slice.ts` (registered in `shared/store/`)
**MobX**; `modules/{feature}/stores/{feature}.store.ts`
**Jotai**; `modules/{feature}/stores/{feature}.atoms.ts`
> Whichever you choose, keep the rules in §4.1 constant. The skill cares that server and UI state are separated and that each module owns one store unit; not which library draws the box.
// modules/invoice/hooks/invoiceKeys.ts; hierarchical key factory (TanStack Query style)
Invalidating `lists()` refreshes every filtered page; `detail(id)` targets one entity. (RTK Query/SWR/Apollo express the same idea with tags/keys.) Components never write raw `fetch()`; they call `useInvoiceList()` / `useCreateInvoice()`.
- **Tamagui (web + native):** a co-located `{name}.styles.ts` exporting `styled(...)` components or a `createStyledContext` / `useStyle` token set; reference Tamagui tokens (`$background`, `$space.4`); never hardcoded values inline. Tamagui is the recommended choice when you target **both web and React Native** from one codebase.
// invoice-list.styles.ts (Tamagui example; works on web AND native)
This skill does not dictate the _visual_ design; pair it with a design/component skill for that. It dictates only _where styling lives_.
- **Interfaces are prefixed with `I`**; `IFeatureUiState`, `IInvoiceListParams`, `IUserProfile`. Type aliases (unions, mapped types, primitives) are **not** prefixed (`type SortDirection = "asc" | "desc"`).
- **Components**: `PascalCase` files and exports; `InvoiceListPage.tsx`, `LineItemRow.tsx`.
- **Pages/screens**: `kebab-case` directories, the component file matches; `pages/invoice-list/invoice-list.tsx`.
- **Hooks**: `useCamelCase`; `useInvoiceList`, `useFeatureUiStore`.
// app/(app)/invoices/page.tsx; thin route file
- "Pages" are "screens"; same directory pattern: `pages/{screen}/{screen}.tsx` + `{screen}.styles.ts`.
// app/invoices/index.tsx (Expo Router); thin screen file
- [ ] Cross-module imports go through the barrel (`@/modules/{feature}`); no deep internal paths.
- [ ] No `fetch()` in components; only typed data hooks built on the shared client.
- [ ] No inline styles; co-located `{name}.styles.ts` (Tailwind/CSS Modules/Tamagui/StyleSheet/styled-components).
- [ ] Routing files are thin; they mount module pages and own only layout/auth boundaries.
**Reviewing structure:** run the checklist in §8. The most valuable catches are state-origin leaks (server data in the client store) and deep cross-module imports (bypassing the barrel); both erode the architecture fastest.
2. Keep the frontmatter `name` and a high-signal `description` (above); that description is what discovery indexes match against.