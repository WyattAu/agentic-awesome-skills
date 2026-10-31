- **Google has already extracted the structured fields you want.** `.knowledge_graph` carries HQ, founder, founded year, parent company, employees, industry. `.organic_results[]` titles and snippets carry person → role → employer mappings (LinkedIn titles are literally `Name; Role at Company`). `.local_results[]` carry phone/address/hours.
### Step 1: SERP for role, employer, LinkedIn URL
  "title": "Jane Doe; Senior Engineer at Acme Corp | LinkedIn",
You now have role, employer, location, LinkedIn URL, and a connection-count hint; without scraping anything. **Stop here unless a specific extra field is required.**
### Step 2: Refine with targeted SERP queries
### Step 3: Web-scraping (only if SERP came up short)
LinkedIn sometimes blocks the public preview; if it does, fall back to step 2 (combining SERP snippets); it's almost always enough.
# 4. Last resort: scrape the company's public contact / about / team pages for emails
### Step 1: SERP knowledge_graph
### Step 2: Targeted SERP for specific fields
### Step 3: Web-scraping (only when SERP can't fill a specific field)
- **Quote names to narrow text matches**; an exact phrase does not uniquely identify a person.
- **Use `site:` aggressively**; `site:linkedin.com/in/`, `site:linkedin.com/company/`, `site:github.com`, `site:crunchbase.com`. Google's `site:` is the cheapest way to scope an enrichment search.
- **AI-extract over CSS selectors** when you do need to scrape; LinkedIn / Crunchbase / About-page markup changes constantly; AI extraction with field names + descriptions survives layout churn.
- **Cross-source verify**; never enrich from a single source. If LinkedIn's title says "Acme Corp" and a `--q '"Jane Doe" "Acme Corp"'` SERP corroborates with multiple results, the results may still repeat the same underlying source; independently verify identity and recency.
- **Mark guesses**; pattern-guessed emails, inferred locations, single-source roles should be flagged to the user as unverified.
- **Respect privacy and authorization**; do not collect or infer personal contact details without a legitimate purpose and user authority.