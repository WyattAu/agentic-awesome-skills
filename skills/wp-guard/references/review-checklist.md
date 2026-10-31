# WP Guard: Review Checklist
- `echo`, `print`, `<?=`, `printf`; every variable escaped with the context-correct `esc_*`/`wp_kses`? (Rule 1)
- `$_POST`, `$_GET`, `$_REQUEST`, `$_SERVER`, `$_COOKIE`; `wp_unslash()` then sanitized? (Rule 2)
- `add_action( 'wp_ajax_`, `admin_post_`, `rest_api_init`; every state-changing handler has BOTH `current_user_can()` and a nonce check / real `permission_callback`? (Rule 3)
- `$wpdb->`; every variable behind `prepare()` placeholders? `esc_like()` for LIKE? (Rule 4)
- `$_FILES`; handled via `wp_handle_upload()` with type allowlist?
- Every hooked hook and called function exists in supported WP/plugin versions? (Rule 6; hallucinated hooks fail silently)
Use the SKILL.md reporting format. Lead with the security findings and an overall verdict (merge / fix first / do not merge). End with at most three positives worth keeping; reviewers who only list faults get ignored.