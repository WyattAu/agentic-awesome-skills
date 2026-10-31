- [Cylect](https://www.cylect.io/); entity extraction + link analysis.
- [Fivecast Matrix](https://www.fivecast.com/products/matrix/); generative-AI triage for social-media datasets.
- [Recorded Future](https://www.recordedfuture.com/); AI-driven threat intel.
- [DarkOwl Vision](https://www.darkowl.com/); darknet data analysis.
- [archive.today](https://archive.today/); one-page archiver + screenshot.
- [URLScan.io](https://urlscan.io/); webpage scan + resource map.
- [ArchiveBox](https://archivebox.io/); self-hosted (HTML, PDF, screenshots, media).
- [Hunchly](https://www.hunch.ly/); investigator evidence capture (paid).
- Wayback SavePageNow API v3; on-demand archiving with job IDs.
- [SingleFileZ](https://github.com/gildas-lormeau/SingleFileZ); browser ext for offline HTML.
- [Kasm Workspaces](https://kasmweb.com/); containerized OSINT browser isolation.
- [n8n](https://n8n.io/); self-hosted workflow automation (RSS → scrape → alert pipelines).
- [Huginn](https://github.com/huginn/huginn); agent-based monitoring/scraping/alerting.
- [Playwright](https://playwright.dev/); headless browser automation without bypassing access controls or platform limits.
- [Browsertrix Crawler](https://github.com/webrecorder/browsertrix-crawler); archival crawling with WARC export.
- [Prefect](https://www.prefect.io/) / [Apache Airflow](https://airflow.apache.org/); workflow orchestration.
   - `<scan>/mobile_endpoints.json`; endpoints + hostnames extracted from APK static analysis.
   - `<scan>/secrets_sidecar.json`; hostnames + endpoints + Firebase project IDs from secrets-beyond-github sweep.
   - `<scan>/sso_tenants.json`; discovered IdP tenants for breach correlation.
## 48. Runnable Helper: `secret_scan.py`
Output is JSONL; one finding per line; drops cleanly into `jq` for filtering or directly into a finding store.
## 49. Runnable Helper: `h1_reference.py`
disclosed reports during recon; no API key required (max 50 results/page). Use it to
## 50. Runnable Helper: `dashboard.py`
`h1_reference.py` behind a stdlib web UI. Binds to `127.0.0.1` by default; the scan
Use it for a fast visual pass; paste a blob or point at a path to aggregate secrets by
severity/category, and browse disclosed-report references; without leaving the box.