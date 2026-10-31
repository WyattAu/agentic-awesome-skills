Use the `hasdata` CLI for real-time web data. One subcommand per API; flags, enums, defaults are derived from the live schema at `api.hasdata.com/apis`.
- `command -v hasdata`; if missing, download the installer from `https://raw.githubusercontent.com/HasData/hasdata-cli/main/install.sh`, inspect it, then run it locally with `sh install.sh`.
- If a call fails with `no API key configured`, the user hasn't run `hasdata configure` yet; tell them to. **Never invent a key.**
| Travel; short-term rentals | `airbnb-listing`, `airbnb-property` |
| Travel; hotels / lodging | `booking-search`, `booking-place` |
| Travel; flights | `google-flights` |
| **Scrape a specific URL** | `web-scraping`; supports JS rendering, proxies, markdown output, AI extraction, screenshots |
- **"Is this still true?" / "What's the latest on X?" / "Has Y happened yet?"**; LLM training data is stale. Run `google-serp` or `google-news` to ground the answer.
- **"Summarize this article" / "TL;DR this URL"**; Use `web-scraping --output-format markdown` and feed the markdown into the summary prompt. Beats copy-paste because it strips ads, nav, scripts.
- **"Verify this link" / "Is this site real?"**; `web-scraping --url X --no-block-resources` returns status + screenshot. Or `google-serp --q "site:example.com"`.
- **"What does X say about itself?"**; Pull the company's own homepage with `web-scraping --output-format markdown`, then summarize.
- **"Find me alternatives to X"**; `google-serp --q "X alternatives"` or `google-shopping --q "X competitors"`.
- **"What's the going rate for X?"**; `google-shopping` (broad) or `amazon-search` (Amazon-specific) with `jq` to extract the price distribution.
- **"Phone number / address for X"**; `google-maps-place` or `yelp-place`. Don't guess from training data.
- **"Are people happy with X service?" / "Is X reputable?"**; `google-maps-reviews --place-id ... --sort lowest` for negative samples; available employer-review data for employer reputation.
- **"What's the salary range for Y role?"**; `indeed-listing` filtered by role + location, then `jq` over `.jobs[].salary`.
- **"Find me homes/apartments matching X criteria"**; `zillow-listing` / `redfin-listing` / `airbnb-listing` with the corresponding filters.
- **"Recent sold comps near X"**; `zillow-listing --type sold --keyword "X" --days-on-zillow 12m`.
- **"Track this product's price"**; Loop `amazon-product --asin X` on a schedule; persist `.price` to a file.
- **"Summarize / cite this YouTube video"**; `youtube-transcript-api --v-param VID --raw | jq -r '.transcript[].snippet'` → feed to the summary prompt. Beats title/thumbnail-based guesses.
- **"Find a hotel in $CITY for $DATES under $BUDGET"**; `booking-search --keyword $CITY --check-in-date X --check-out-date Y --adults 2 --children 0 --rooms 1 --price-max $BUDGET --sort priceLowestFirst`. For one specific property, `booking-place --url ...` returns the full room/rate matrix.
- **"What's this channel pushing lately?"**; `youtube-channel-api --channel-id @handle --tab videos --raw | jq '.sections[].items[] | {title, publishedDate, views: .extractedViews}'`.
- **"Does this business have an active offer / event?"**; `google-maps-posts --place-id X --raw | jq '.posts[] | {postedAt, description, cta}'`. Surfaces current promotions Google indexed.
- **"What's trending around X?"**; `google-trends --q "X"` for relative interest; `google-news --q "X"` for headlines.
- **"Find businesses near me that do X"**; `google-maps --q "X" --ll "@LAT,LNG,12z"` then fan out `google-maps-place` for contacts.
- **"How does this look in country Y?"**; `--gl Y` on SERP commands, `--proxy-country Y` on `web-scraping`. Useful for geo-targeted SEO checks, geo-blocked content.
- **"Pull structured data from this page"**; `web-scraping --ai-extract-rules-json '{"price": {"type": "number"}, ...}'`. Works on arbitrary pages without writing CSS selectors.
- **"List of items → per-item details"**; Pattern: search command produces IDs/URLs, pipe through `xargs` into the matching `*-property` / `*-product` / `*-place` deep-dive command.
- **"Find this person's role / employer / LinkedIn / followers"**; `google-serp --q '"Person Name" linkedin'` first. The organic-result title is typically `Name; Role at Company | LinkedIn` and the snippet carries location, headline, connection count. SERP often answers the whole question without ever opening the profile page.
- **"What is company X doing? Where's their HQ? Who works there?"**; `google-serp --q "$COMPANY"` returns a `.knowledge_graph` block with founder, HQ, founded year, parent, employee range; pre-extracted. `google-news --q "$COMPANY"` for recent activity. Specific facts via targeted SERP: `--q '"$COMPANY" headquarters'`, `--q '"$COMPANY" funding'`, `--q 'site:linkedin.com/company "$COMPANY"'`.
- **"Find public contact channels for company X"**; start with SERP: `--q '"@example.com"'` often surfaces publicly indexed business addresses. For personal emails or phone numbers, require a legitimate purpose, user authorization, and privacy-law/terms compliance; disclose unverified guesses.
- **"Enrich this CSV of leads"**; per row: `google-serp` for LinkedIn, role, employer; another SERP to verify email or pattern. Stay in SERP unless a specific field is missing.
- **Reverse-lookup (email / phone / domain → identity)**; `google-serp` with the literal value in quotes (`--q '"jane@x.com"'`, `--q '"+1 555 123 4567"'`, `--q '"acme corp" site:example.com'`) can surface possible matches; disambiguate identity and confirm with independent sources.
- **Enum flags** validate client-side. If you guess wrong, the error lists the allowed values; read the message and retry.
For real-estate / e-commerce results, the array shape is API-specific; read a single response with `--pretty` first to learn the schema, then write the `jq` filter.
- [`references/enrichment.md`](references/enrichment.md); **person and company enrichment** (LinkedIn lookup, emails, HQ/funding/news, CSV-row enrichment, reverse-lookup); the highest-leverage cross-API workflows
- [`references/search.md`](references/search.md); Google SERP / Bing / News / Trends flag catalog
- [`references/web-scraping.md`](references/web-scraping.md); `web-scraping` flags, JS scenarios, AI extraction
- [`references/real-estate.md`](references/real-estate.md); Zillow / Redfin filters and bracketed params
- [`references/travel.md`](references/travel.md); Airbnb / Booking / Google Flights (lodging + transport)
- [`references/ecommerce.md`](references/ecommerce.md); Amazon / Shopify
- [`references/local-business.md`](references/local-business.md); Maps (search/place/reviews/photos/posts) / Yelp / YellowPages
- [`references/jobs.md`](references/jobs.md); Indeed / Glassdoor
- [`references/youtube.md`](references/youtube.md); search / video / channel / transcript
- [`references/all-commands.md`](references/all-commands.md); full subcommand index with credit costs