> Portable skill; readable by Claude Code, OpenCode, Codex, Cursor, Windsurf, and others.
> This skill describes the **discipline of the write path**; optimistic updates, rollback,
> idempotency, cache coherence; not a UI library or a styling system. It builds directly on the
**coherent** (the detail view and every list page agree). All three at once; that's the craft.
2. **Roll back verbatim.** On failure, restore the exact snapshot taken before the patch; not a "best guess" re-derivation. Keep the snapshot in mutation context.
| High-confidence, low-conflict write (toggle nonfinancial status, like, reorder) | **Optimistic**; patch immediately, roll back on error.                                                                        |
| Destructive or hard-to-reverse write (delete with cascade, send money)        | **Confirm first**, then optimistic _or_ pending; never silent-optimistic.                                                     |
- **cancel**; without it, a query that was already in flight can resolve _after_ your patch and overwrite the optimistic state.
- **snapshot**; the only safe rollback source; never reconstruct prior state by hand.
- **patch**; the instant UX; mutate detail **and** lists together (§4).
- **failure**; revalidate and surface the typed ApiError; this concurrent example deliberately does not restore whole-cache snapshots. Use pending UI while authoritative data reloads.
- **invalidate on settle**; success or failure, refetch so server-computed fields (timestamps, totals) are authoritative. Settle, not just success: a failed write may still have changed server state.
// at form initialisation; stable for the lifetime of this attempt
- **Reads:** retry a few times with backoff (default in most query libs); safe and idempotent.
- **Conflicts (409):** don't retry; surface the typed error, invalidate, and let the user re-decide on fresh data.
| **SWR**            | `mutate(key, optimisticData, { rollbackOnError: true, populateCache, revalidate: true })`; optimistic data + automatic rollback + revalidate.             |
- [ ] Server state stays in the query cache; never copied into a client store.
2. Keep the frontmatter `name` and high-signal `description`; discovery indexes match against it.