# Web Scraping API: `POST /scrape/web`
print(resp.text)            # raw markdown; no JSON parsing
| `proxyType` | enum | `datacenter` (default) or `residential`; use residential only for authorized geo/availability testing where terms and access controls permit it. |
| `proxyCountry` | string | ISO 3166-1 alpha-2; `US`, `UK`, `DE`, `FR`, `IT`, `SE`, `BR`, `CA`, `JP`, `SG`, `IN`, `ID`, `IE`. |
| `jsRendering` | bool | Headless browser; required for SPAs and dynamically-injected content. |
| `headers` | object | Custom headers. **Cookies go here too; no separate `cookies` parameter.** |
| `blockResources` / `blockAds` | bool | Skip images/CSS/ads; speeds text-only scrapes. |
Actions: `click`, `fill: [sel, val]`, `wait: ms`, `waitFor: sel`, `waitForAndClick: sel`, `scrollX/scrollY: px`, `evaluate: "JS"`. Sequential. Missing element on `click`/`fill` fails the request; wrap with `waitFor` first.
The wrapper is JSON **only when** the response is JSON-wrapped; i.e. multiple `outputFormat` values, or a single value that includes `"json"`. With a single non-JSON format the response body is the raw content (`text/markdown`, `text/html`, `text/plain`).
- **Disable `jsRendering` first**, enable only when the page needs it; most static pages parse fine without a headless browser.
- **`extractRules` returns first match**; for arrays use `aiExtractRules` `type: "list"`.