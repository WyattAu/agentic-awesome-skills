| **Web Scraping API** | seconds | Arbitrary URL; JS rendering, CSS/AI extraction, screenshots | `POST /scrape/web` |
**Decision rule.** Default to a **Scraper API** when one exists for the platform (pre-parsed JSON, no selector maintenance). Use **Web Scraping** for arbitrary URLs not covered by an API. Reach for a **Scraper Job** only when no API equivalent exists; `crawler`, `contacts`, `sec-edgar`, `amazon-bestsellers`, `amazon-product-reviews`; *or* when async fan-out + webhooks save engineering time over a paginated client loop.
- **Retries:** `429` and `5xx` only; exponential backoff, jitter. Never retry other `4xx` (auth, validation).
- **Async jobs:** the submit response handle is `body.id` (integer), **not `jobId`**. Persist it immediately. Poll `GET /scrapers/jobs/<id>` every 10–30 s with backoff; treat webhooks as best-effort and always pair with polling. On `finished` the status carries `data: {csv, json, xlsx}` short-lived URLs; download immediately.
- **Disable `jsRendering` first**, enable only if the page needs it; most static pages parse fine without a headless browser.
- **No `cookies` parameter**; cookies go through `headers["Cookie"]`.
- [`references/web-scraping.md`](references/web-scraping.md); `POST /scrape/web` parameters, JS scenarios, AI extraction, cookie auth.
- [`references/search.md`](references/search.md); Google SERP / Light / AI Mode / News / Shopping / Bing / Trends + pagination.
- [`references/ecommerce.md`](references/ecommerce.md); Amazon (product, search, seller, seller-products) and Shopify.
- [`references/real-estate.md`](references/real-estate.md); Zillow, Redfin (bracketed filters).
- [`references/travel.md`](references/travel.md); Airbnb, Booking, Google Flights (occupancy rules, token pagination, IATA codes).
- [`references/local-business.md`](references/local-business.md); Maps (search/place/reviews/photos/posts), Yelp, YellowPages.
- [`references/jobs.md`](references/jobs.md); Indeed and Glassdoor.
- [`references/youtube.md`](references/youtube.md); YouTube search / video / channel / transcript.
- [`references/scraper-jobs.md`](references/scraper-jobs.md); async submit/poll/results, Crawler, Contacts, SEC EDGAR, webhook receiver.
- [`references/code-recipes.md`](references/code-recipes.md); Python / TypeScript clients with retry, backoff, concurrency, polling.