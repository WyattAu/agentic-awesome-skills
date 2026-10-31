# WP Guard: Security Reference
| Data for inline JS | `wp_json_encode()` + `wp_add_inline_script()`; `esc_js()` is legacy; single-quoted strings in inline attributes only |
| Translation + output | `esc_html__()`, `esc_html_e()`, `esc_attr__()`; escape and translate in one call |
Trust nothing at output time; not even your own stored options; another plugin or a compromised import may have written them. "Escaped on save" is not a defense.
- `current_user_can( 'manage_options' )`; is this user *allowed* to do this? (authorization)
- `check_admin_referer( 'ncs_save_settings' )` / `check_ajax_referer( 'ncs_action', 'nonce' )`; did this request *intend* this action? (CSRF protection)
A nonce without a capability check lets any logged-in subscriber fire admin actions. A capability check without a nonce leaves CSRF open. AI-generated handlers routinely have one, the other, or neither; verify both exist on every `admin_post_*`, `wp_ajax_*`, and form handler.
- Every route registers a real `permission_callback`. `__return_true` is acceptable only for genuinely public read-only data; never for writes.
- `IN ( … )` lists: build placeholders dynamically; `implode( ',', array_fill( 0, count( $ids ), '%d' ) )`; then prepare with the spread array.
- Prefer `WP_Query`, `get_posts()`, meta/term/option APIs when they can express the query; they bring caching for free.
Use `wp_handle_upload()` / media APIs; never move `$_FILES` manually. Validate type with `wp_check_filetype_and_ext()` against an allowlist; never trust the client MIME. The API alone does not guarantee private storage or a server execution ban. Enforce upload authorization, size/type restrictions, web-server non-execution and private access controls where needed.
1. `echo '<div>' . $title . '</div>';`; unescaped output (Rule 1). Published research: XSS appears in 86% of AI generations tested on XSS-prone tasks (see [sources.md](sources.md)).
3. `"SELECT * FROM {$wpdb->posts} WHERE post_title = '$title'"`; interpolated SQL (Rule 4).
5. `curl_init()` inside a plugin (Rule 5); breaks proxies, blocks, and filters that `wp_remote_*` honors.
6. Echoed `<script>` blocks with interpolated PHP; combines Rules 1 and 5; use `wp_enqueue_script()` + `wp_add_inline_script()`/`wp_localize_script()`.