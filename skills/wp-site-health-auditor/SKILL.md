prioritized, risk-tiered fix plan; then executes the safe fixes and hands off the rest with exact
## ⚠️ Safety: read before touching any file
   user before running it for real. This command rewrites the database in place; a wrong pattern can
   the next change. Don't batch multiple Tier 2 file edits into one pass; if something breaks, you want to
   State this even if nothing goes wrong; it costs one line and saves a panicked user later.
If the user says "just do it, skip the backup"; still create the backup silently as part of the edit
backend) or are purely informational (SQL server version; no action needed). This skill sorts that out -
## Phase 1: Parse the report
- WP-CLI output (`wp site-health check` is not a core command; note that up front, don't invent one; see Phase 4)
1. **Critical issues** (red); always fix first, always confirm before touching.
2. **Recommended improvements** (yellow); the bulk of real work; triage by risk tier below.
3. **Passed tests** (green); skip. Do not "fix" or re-verify passed tests unless the user asks. Do not
   invent problems with green items; a common failure mode is treating "SQL server is up to date" as
verbatim before triaging; don't paraphrase the WordPress-generated title, it's used for the fix lookup in
text (fastest) rather than guessing; Site Health results are host- and config-specific and guessing wastes
## Phase 2: Risk-tiered triage
to the user first for anything above Tier 1 count of 3+ items; don't silently start deactivating plugins.
**Tier 1; Safe, reversible, auto-fixable in wp-admin or via WP-CLI**
- Remove inactive plugins/themes (they aren't running, deactivation already happened; this is just
**Tier 2; Requires host/server-level access; Claude drafts the change, user or host applies it**
- Permalink structure change (migration; existing URLs break without redirects; require a redirect plan and CDN/cache flush before applying)
**Tier 3; Informational / host-dependent, no fix exists or none needed**
## Phase 3: Fix recipes by item
match anything here, say so explicitly rather than fabricating a fix; Site Health's item set changes
### You should remove inactive plugins / themes: Tier 1
Twenty Twenty-Five (or the current default core theme) if it's the only fallback theme; WordPress needs
Confirm the exact plugin/theme names with the user before deleting; inactive isn't the same as unused;
### post_max_size smaller than upload_max_filesize: Tier 2
1. Host control panel PHP settings (cPanel "Select PHP Version" > Options, Plesk, etc.); no code needed,
2. `php.ini` (if the user has server access); back up first (`cp php.ini php.ini.bak-<timestamp>`):
3. `.htaccess` (Apache + mod_php only, not on PHP-FPM/nginx); back up first:
4. `.user.ini` (CGI/FastCGI hosts; not mod_php); back up first, create or edit `.user.ini`
   ⚠️ **Do not use `ini_set()` in `wp-config.php` for these directives**; `upload_max_filesize`
(`wp cli info` doesn't show these; use the authenticated host panel or private diagnostic output) rather than assuming defaults.
### You should use a persistent object cache: Tier 2
Requires a caching backend (Redis or Memcached) installed at the server level; this is not something a
   an `object-cache.php` drop-in to `wp-content/`; confirm no existing `object-cache.php` is being
3. If not available, this is a hosting-tier limitation; report it as such rather than trying to fake a
### Page cache is not detected: Tier 2
   active but not reporting the headers Site Health looks for; worth confirming with the host before
2. If not, install one page-cache plugin (not a full plugin stack); WP Super Cache, W3 Total Cache, or
   settings screen (no reliable WP-CLI toggle across cache plugins; flag manual step to user).
   status page before adding another. Some cache plugins also write rules into `.htaccess`; back it up
### Your site is not set to output debug information: usually already passing; if failing: Tier 1
### REST API / loopback requests / background updates failing: Tier 2
### HTTPS not fully active: Tier 2
## Phase 4: What NOT to invent
- There is no `wp site-health` WP-CLI command in WordPress core as of this writing; don't fabricate one.
- Don't claim a fix is complete without the user (or a re-run of Site Health) confirming it; server-level
- Don't guess PHP/server values (current `upload_max_filesize`, cache backend availability, etc.); ask or
- Don't skip or shortcut the Safety section for any reason, including "it's a small change"; file
## Phase 5: Output format
Keep the whole response scannable; this is a punch list, not an essay. Use the table + short recipe
- ✅ Back up the specific file before every edit; `cp` takes seconds, restoring a dead site takes hours
- ❌ Never batch multiple Tier-2 file edits into one pass; you won't know which change broke the site
`references/catalog.md`; extended list of less-common Site Health items (SEO category items like llms.txt
- **Treating every yellow item as actionable**; Some recommended improvements (e.g. persistent object cache) are host-level and may not be fixable. Always triage by tier before acting.
- **Changing permalinks without a redirect plan**; Flipping to "Post name" on an indexed site breaks every existing URL. Always plan 301 redirects first.
- **Using `ini_set()` for upload limits**; `upload_max_filesize` and `post_max_size` are `PHP_INI_PERDIR`; `ini_set()` silently fails. Use `php.ini`, `.htaccess`, or `.user.ini` instead.
- **Skipping the dry-run on `wp search-replace`**; A wrong pattern can corrupt serialized data. Never run it without `--dry-run` first.
- **Installing two caching plugins**; Stacking page cache plugins causes conflicts and obscure bugs. If one is already active but not detected, debug it rather than adding another.
- Cannot execute anything itself against a live site; every WP-CLI/PHP snippet is drafted for the user or
- Cannot verify current PHP/server values (upload limits, cache backend availability, HTTPS status) -
  mid-2026; item titles/wording can change across core versions, so an unmatched item should be reported
- Does not replace a full security audit or compromise scan; Site Health flags configuration hygiene
- `@security-hardening`; For deeper WordPress security audits beyond Site Health's surface checks
- `@wp-performance`; For targeted performance optimization after Site Health flags are resolved