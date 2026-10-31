# WP Guard: Performance Reference
- `query_posts()` is forbidden; it clobbers the main query. Use `WP_Query` or `pre_get_posts`.
- Avoid `'meta_query'` on unindexed scans for high-traffic paths; meta queries do not scale on large `postmeta` tables; consider a lookup table or taxonomy when the access pattern is hot.
- `set_transient()` / `get_transient()`; persistent cache with TTL; backed by the object cache when a drop-in exists, by the options table otherwise.
- `wp_cache_get()` / `wp_cache_set()`; request-scope by default, persistent only with an object-cache drop-in (Redis/Memcached). Use cache groups and deliberate TTLs.
- Expensive remote calls (Rule 11) always go behind one of these. Choose TTL consciously; never cache user-specific data in a shared key; include the user/locale in the key when output varies by them.
- On failure paths, decide explicitly whether to cache the failure (short TTL) or retry every request; and say which in a comment.
- `add_option( 'ncs_big_report_cache', $data, '', false )`; large or rarely-read data never autoloads (pass `false` as the third arg of `update_option` too; the parameter exists since WP 4.2).
- Unbounded `get_users()` / `get_terms()` on large sites; same rules as posts.
- Per-request remote HTTP without caching; one slow third party becomes your TTFB.