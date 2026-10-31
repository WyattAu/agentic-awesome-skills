- `--url URL`; target page
- `--js-rendering` / `--no-js-rendering` (default on); full headless browser
- `--block-resources` / `--no-block-resources` (default on); blocks images/CSS for speed
- `--remove-base64-images`; strip inline base64 images from response
- `--wait MS`; fixed wait after page load
- `--wait-for "CSS_SELECTOR"`; wait until selector appears
## Custom JS scenario (complex array: JSON only)
Supported types: `string`, `number`, `boolean`, `list`, `item` (nested object; defines its shape under `output`).
- `--include-only-tags "main,article"` (comma-joined CSS selectors); keep only matching elements
- `--exclude-tags script --exclude-tags style` (repeatable); remove elements
The `web-scraping` response is JSON, but if `--output-format` is set to a single non-JSON format, the wrapped result is still JSON. Use `jq -r .markdown > file.md` to extract text. For screenshots specifically, the response contains a screenshot URL; fetch it separately with `curl`.
- **Page-to-prompt grounding**; `--output-format markdown` produces clean LLM-ready text from any URL. Strip nav/ads with `--exclude-tags script --exclude-tags style --exclude-tags nav`. Beats fetch + regex.
- **JavaScript-rendered SPAs that `curl` can't read**; default `--js-rendering` uses a real browser, so React/Vue/Angular pages return their hydrated DOM, not the empty shell.
- **Geo/availability testing where allowed**; `--proxy-type residential` can model residential network availability; use only for authorized tests where the target's terms and access controls permit it.
- **Geo-targeted content**; `--proxy-country DE` to see what users in Germany see (different prices, currencies, A/B variants, or geo-blocked content).
- **Quick "is this page real" check**; `--screenshot` (default on) returns a screenshot URL in the response; verify visually without manually opening the URL.
- **Universal price extractor**; `--ai-extract-rules-json '{"price":{"type":"number"},"currency":{"type":"string"},"in_stock":{"type":"boolean"}}'` works on arbitrary retailer pages without writing a CSS selector. Cheaper than maintaining per-site selectors when the user only needs occasional spot-checks.
- **Authenticated content with user authority**; `--headers Cookie=session=...` injects auth cookies if the user has them. Use only with explicit permission and authority to access that account/content; never use cookies to bypass someone else's access controls.
- **Convert paginated lists to a clean record set**; combine `--js-scenario-json` (click "Load more" 5×) with `--ai-extract-rules-json` (pull the list shape). Lets you scrape paginated SPAs with one CLI call instead of N.
- **Headless screenshot of a layout**; set `--js-rendering`, `--no-block-resources` (so CSS loads), and capture the screenshot URL from the response. Useful for "render this URL and show me what it looks like".
- **Markdown for RAG ingestion**; pipe `.markdown` from many URLs into a JSONL corpus; embed and store. The CLI handles JS, ads, images so you don't need a custom pipeline.
- **Fallback for any other API**; when no purpose-built API exists for a vertical (e.g. niche directories, government pages, less-popular real-estate sites), `web-scraping` is the catch-all.
- **Detect content changes**; schedule `web-scraping --url X --output-format markdown` and diff the output across runs to flag pricing-page or terms-of-service changes.
- **Read PDFs / non-HTML resources**; `--output-format text` works on text-extractable PDFs accessible via URL (the underlying renderer handles them).
- **AI extraction for forms or tables**; pages with structured data in HTML tables are easy: `--ai-extract-rules-json '{"rows":{"type":"list","output":{"name":{"type":"string"},"value":{"type":"number"}}}}'`. The model fills in nested rows.