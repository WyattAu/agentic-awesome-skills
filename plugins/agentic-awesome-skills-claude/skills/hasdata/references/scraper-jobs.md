# Scraper Jobs: async, bulk
1. `POST /scrapers/<slug>/jobs` → returns the full job record. **The handle is `body.id` (numeric integer), not `jobId`** despite older doc snippets; store this. Status starts as `pending`.
2. `GET /scrapers/jobs/<id>`; poll status.
3. `GET /scrapers/jobs/<id>/results?page=…&limit=100`; once `status === "finished"`.
4. `DELETE /scrapers/jobs/<id>`; stop early (rows produced before stop are kept).
For one-shot ingestion, fetch `data.json` directly instead of paging `/results`. **These URLs are short-lived**; download immediately on `finished`.
    return r.json()["id"]                            # numeric job id; not "jobId"
            yield row["data"]                       # double-wrapped; see below
**Double `data`**; the row is `body["data"][i]["data"]`; the outer wraps with `id`, `jobId`, `dataId`, `createdAt`, `updatedAt`.
- `limit` (int); max rows. `0` = no cap.
- `webhook.url` (string, https), `webhook.events` (any subset of `scraper.job.started`, `scraper.data.scraped`, `scraper.job.finished`), `webhook.headers` (sent on every callback; pin a shared secret here).
- Async with **3 retries** on non-2xx. **Order not guaranteed**; payload is the source of truth.
### `crawler`: recursive site crawl
### `contacts`: URLs → contact info
  "xcom":         ["..."],          // X / Twitter; note key is "xcom"
Empty arrays for missing categories; never null. If you only have a domain, discover URLs first via SERP `site:example.com`.
### `sec-edgar`: bulk SEC filings
- **crawler**; unknown URL set, recursive discovery.
- **contacts**; known URL list, want extracted contact fields.
- **`/scrape/batch/web`**; known URL list, want full HTML/markdown/AI extraction at >1k scale.
- **Persist the job `id` immediately** (the integer from the submit response; *not* `jobId`). Only handle to status, results, stop.
- **Don't poll faster than every 10 s**; wastes concurrency cap.
- **Double-wrapped results**; `body["data"][i]["data"]`, not `body["data"][i]`.