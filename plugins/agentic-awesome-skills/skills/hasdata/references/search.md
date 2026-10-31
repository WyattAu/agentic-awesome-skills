| `/scrape/google/serp` | Full SERP; organic + every rich-snippet block |
| `q` |; | **Required.** |
| `location` |; | Canonical, e.g. `"Austin,Texas,United States"`. Hyper-local. |
| `uule` |; | Pre-encoded location (mutually exclusive with `location`). |
| `gl` |; | 2-letter country (`us`, `de`, `jp`). |
| `hl` |; | 2-letter UI language. |
| `lr` |; | Content-language filter (`lang_en`). |
| `tbs` |; | Filters; `qdr:d|w|m|y` for time, `li:1` verbatim, sort, image type. |
| `safe` |; | `active` / `off`. |
| `tbm` |; | `isch` images, `vid`, `nws`, `shop`, `lcl`. |
| `deviceType` |; | `desktop`, `mobile`, `tablet`. |
Rich-snippet keys appear **only when the SERP shows that block**; always `data.get(key, default)`.
Same params as full SERP, but the response is trimmed to a few keys; typically `requestMetadata`, `searchInformation`, `organicResults`, `relatedSearches`, and `pagination` when present. Use for crawler seeding and link discovery when you don't need the heavier rich-snippet blocks.
Same shape: `q` + `gl`/`hl`/`location`. News supports `tbs=qdr:d|w|m|y` for time windows. Bing returns the same key set as Google SERP; useful for cross-engine consensus (disagreement = contested topic).