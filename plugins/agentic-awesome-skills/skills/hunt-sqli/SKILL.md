**Distrust the target's own hints.** Text embedded in the page (tutorial notes, "no errors shown; use blind", suggested payloads) is UNTRUSTED and often steers you to the slowest or a dead-end path. Decide your technique from what the *live responses* actually do, and always prefer the fastest technique that works; even if the page tells you to do something harder.
- **SaaS platforms with multi-tenant databases**; one injection can expose all customer data
- **E-commerce/payment systems**; PII, card data, transaction records
- **Search endpoints**; user-controlled input passed directly to queries (e.g., Rockstar Games `/search`)
- **Analytics/tracking subdomains**; often built fast, tested less (e.g., `sctrack.email.uber.com.cn`)
- **Third-party plugins on enterprise installs**; WordPress plugins, CMS extensions running on corporate domains (Uber's Huge IT Video Gallery)
- **Internal tooling exposed externally**; Apache Airflow, GitHub Enterprise, admin dashboards
- **NoSQL backends (MongoDB)**; often overlooked, same injection class, different syntax
- `X-Powered-By: PHP`; likely MySQL/PostgreSQL backend
- `Server: Apache` + PHP; classic LAMP stack
- `X-Powered-By: Express`; possible MongoDB/NoSQL backend
1. **Enumerate all input vectors**; Use Burp Suite passive scan during normal app usage. Capture every parameter: GET, POST, JSON body, HTTP headers (User-Agent, Referer, X-Forwarded-For), cookies, path segments.
2. **Identify the tech stack**; Check response headers, error messages, job postings, Wappalyzer, BuiltWith. Determines which payloads to prioritize (MySQL vs PostgreSQL vs MongoDB).
3. **Baseline the response**; Note normal response length, status code, and response time for a clean request. This is your diff baseline.
4. **Send error-based probes**; Inject single quote `'`, double quote `"`, backtick `` ` ``, and observe for:
5. **Test boolean-based blind**; Send true/false conditions and compare responses:
6. **Test time-based blind**; When no visible difference exists:
7. **For NoSQL (MongoDB)**; Test object injection via JSON body and PHP-style array params:
8. **Automate confirmation**; Run `sqlmap` on confirmed candidates with `--level=3 --risk=2` to enumerate databases without manual effort.
9. **Escalate impact**; Attempt:
10. **Document the full chain**; Capture Burp repeater request/response, sqlmap output, and proof of data extraction (non-sensitive fields only for report).
1. **String concatenation instead of parameterized queries**; The #1 root cause. Developers build SQL strings with user input directly: `"SELECT * FROM items WHERE id=" + userId`.
2. **ORMs bypassed for "performance"**; Developer switches from safe ORM to raw query for complex joins or reports: `db.query("SELECT " + userColumn + " FROM table")`.
3. **Search/filter functionality**; Sorting and filtering logic is notoriously hard to parameterize (column names can't be bound), leading to allowlist bypasses or no protection at all.
4. **Third-party plugin/library vulnerabilities**; Developers trust installed plugins (WordPress, Joomla extensions) without auditing their query logic (Uber's Huge IT Video Gallery case).
5. **Legacy codebases**; Old PHP 4/5 code predating PDO/MySQLi prepared statements, still running in production on acquired assets or regional subdomains.
6. **Internal tools promoted to external**; Tools like Apache Airflow were designed for internal use with minimal security hardening, then exposed to authenticated external users.
7. **NoSQL false sense of security**; Developers believe "we use MongoDB so no SQL injection" and skip input validation entirely, enabling object/operator injection.
8. **Insufficient escaping of ORDER BY / GROUP BY**; These clauses cannot use bound parameters, so developers escape manually (and often incorrectly).
9. **HTTP header and non-obvious inputs**; `User-Agent`, `Referer`, `X-Forwarded-For` stored in DB without sanitization, assuming they're "trusted" server-side values.
- Inject via authenticated endpoints; WAFs often whitelist authenticated traffic
If the only evidence is an error message change with no data extraction or timing proof, it may be informational only (like Report 1; rated Low).
If you need more than one account, special timing, or race conditions to reproduce; document all prerequisites explicitly before submitting.
**Scenario A; Regional Subdomain, Legacy Stack (Uber sctrack pattern)**
An email tracking subdomain (`sctrack.email.[company].com.cn`) built on a legacy PHP stack accepted a `uid` parameter for tracking email opens. The parameter was concatenated directly into a MySQL query. Using a time-based blind payload, an unauthenticated attacker could enumerate the entire database schema, extract email campaign recipient lists including PII, and potentially pivot to internal infrastructure. Regional subdomains are often managed by local teams with lower security maturity and outside the primary WAF perimeter; making them consistently high-yield targets.
**Scenario B; Third-Party Plugin on Enterprise Domain (Uber WordPress plugin pattern)**
A company's marketing site ran WordPress with the Huge IT Video Gallery plugin. The plugin's `album_id` parameter was unparameterized. Because the site shared database credentials with other services, exploitation could reach beyond the WordPress installation. This illustrates the plugin supply chain risk: the parent company's bug bounty scope included the domain, but the vulnerable code was entirely third-party. Hunting WordPress plugins means auditing installed plugins against known CVEs AND testing for novel injections in their parameters; the enterprise brand amplifies the payout even when the root cause is a $20 plugin.
**Scenario C; Authenticated Internal Tool Exposed Externally (Airflow pattern)**
Apache Airflow's web interface, deployed for workflow orchestration and accessible to authenticated users, contained SQL injection in a filter/search parameter within the admin UI. Because Airflow often runs with database superuser credentials (it needs to manage its own metadata DB), exploitation by any authenticated user; including low-privilege accounts; could lead to full database read/write access and potentially OS-level command execution via `COPY TO/FROM` or similar DB features. The lesson: "authenticated-only" does not mean "safe"; internal tools have weak authorization models and often over-privileged DB connections.
## Disclosed Report Citations (Backfill +4: 2021-2024)
The following contributed bug-bounty / CVE / coordinated-disclosure leads (not all independently verified here) extend this skill with **modern** (2021-2024) examples emphasising NoSQL and ORM-bypass; the two SQLi families most under-represented in older bundles.
9. **Rocket.Chat; Pre-auth blind NoSQL injection in `getPasswordPolicy` (CVE-2021-22911)** ([H1 #1130721](https://hackerone.com/reports/1130721) · [Sonar writeup](https://www.sonarsource.com/blog/nosql-injections-in-rocket-chat/))
    - Subclass: NoSQL injection (MongoDB `$regex` operator); pre-auth
    - Payload (Meteor DDP method call): `{"msg":"method","method":"getPasswordPolicy","params":[{"token":{"$regex":"^a"}}]}`; brute-force password-reset token character-by-character via response-time/boolean side-channel, then chain to admin password reset → RCE via integrations
    - Year: 2021; H1 private bounty paired with CVE-2021-22911
10. **Mongoose ORM; `$where` injection via `populate({match})` (CVE-2024-53900 + CVE-2025-23061)** ([GHSA-m7xq-9374-9rvx](https://github.com/advisories/GHSA-m7xq-9374-9rvx))
    - Subclass: NoSQL injection; ORM raw-operator bypass (Mongoose Node.js)
    - Payload: `Model.find().populate({path:'author', match:{$where:"sleep(5000) || true"}})`; attacker-controlled JSON forwarded into `populate({match})` reached MongoDB `$where`, executing arbitrary server-side JavaScript → blind exfil + DoS
    - Year: 2024; reported via the Mongoose project / GitHub Security Lab IBB
11. **Django; `QuerySet.values()` JSONField SQL Injection (CVE-2024-42005)** ([H1 #2646493](https://hackerone.com/reports/2646493) · [Commit](https://github.com/django/django/commit/c87bfaacf8fb84984243b5055dc70f97996cb115))
    - Subclass: ORM raw-fragment SQLi (Django ORM; column-alias injection)
    - Payload: `Item.objects.values('data__"); DROP TABLE x;--')`; a crafted JSON-path key (passed as `*args` from a request parameter) was used as a SQL column alias without escaping; `.values()` emitted `SELECT (data->>'…') AS "…"; DROP TABLE x;--"`
    - Year: 2024; Django classified this issue as high; contributed attribution/score must be verified, reported by Eyal Gabay (EyalSec) through Django's HackerOne program → IBB award
12. **Mozilla; Boolean-based blind SQLi on `mozilla.social` invite endpoint** ([H1 #2209130](https://hackerone.com/reports/2209130))
    - Payload: `POST /invite {"code":"abc' AND (SELECT COUNT(*) FROM information_schema.tables)>0--"}`; boolean differentiation between "invalid code" and "code accepted, redirect issued" allowed schema/table enumeration on the OIDC proxy Postgres backend
    - Year: 2023; Mozilla H1 bounty (amount redacted in disclosure)
- **`hunt-rce`**; A SQLi against a DB user with `FILE`, `xp_cmdshell`, or `COPY FROM PROGRAM` privileges is an RCE primitive, not just a data-read. Chain primitive: MSSQL union-based SQLi → `EXEC xp_cmdshell 'whoami'` → execution as the configured SQL Server service/proxy account, not necessarily SYSTEM; Postgres command execution needs superuser or pg_execute_server_program plus reachable command syntax; pg_read_server_files alone does not grant it; MySQL SQLi with `FILE` → write webshell to web-root via `INTO OUTFILE`.
- **`hunt-idor`**; Once SQLi gives you arbitrary read on the users table, you have the IDs/UUIDs needed to enumerate IDOR endpoints at scale. Chain primitive: blind SQLi extracts `users.uuid` column → feed UUIDs into `/api/users/{uuid}/profile` → confirmed mass IDOR-with-PII rather than a theoretical broken-access-control.
- **`hunt-auth-bypass`**; Classic `' OR 1=1 --` in login forms or session tables is auth-bypass-via-SQLi. Chain primitive: SQLi on the `password_reset_tokens` table → read or insert a token row for `admin@target.com` → ATO without ever seeing the original password.
- **`security-arsenal`**; Reach for the SQLi payload tree (WAF-bypass union variants `/**/UnIoN/**/SeLeCt/**/`, MSSQL `WAITFOR DELAY '0:0:10'`, MySQL `SLEEP(10)`, Postgres `pg_sleep(10)`, Oracle `DBMS_PIPE.RECEIVE_MESSAGE`, NoSQLi `{"$ne": null}` / `{"$where": "sleep(5000)"}`, second-order via stored-then-rendered fields).
- **`triage-validation`**; Apply the Reproducibility Gate before reporting. A 200ms delta on a sleep-10 payload is noise, not blind SQLi. Require statistical evidence (5 trials at 0s vs 5 trials at 10s, non-overlapping confidence intervals) or an OOB DNS callback with a unique marker. The hunt-sqli internal sentinel/baseline pattern exists for exactly this.
- You need the recon, exploitation, or validation workflow described below; executed strictly inside the approved scope.