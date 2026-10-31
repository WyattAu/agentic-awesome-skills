### Password policy review: not inferred from session metadata
### The site is not using a persistent connection (not an object cache): Tier 3
### File permissions should be reviewed: Tier 2
### Indexed autoloaded options found: Tier 2
- `wp option delete <key>` (only if the option is safe to delete; confirm
### Can not process scheduled maintenance jobs (wp_cron): Tier 2
### Background updates failed: Tier 2
### llms.txt: optional external/plugin concern, not a core Site Health fix
### No XML sitemap detected: Tier 1
### Social preview images missing: Tier 2
### Privacy policy page not set: Tier 1
### Privacy policy page is outdated: Tier 2
command for this; manual step.
### Requirement mis-match with Xdebug extension: Tier 3
### ZipArchive / gzinflate / BCMath / Imagick missing: Tier 2
(Command varies by OS; draft the correct one for the user's environment.)
- **SQL Server version**; WP core already reports this as Passed or
- **Active theme up to date**; Already green in Site Health; no action.
- **Plugin updates available**; Handled by `wp plugin update --all`.