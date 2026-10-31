# Local Business APIs: Google Maps, Yelp, YellowPages
| `ll` | `@LAT,LNG,ZOOMz` viewport; **lat/lng + zoom, not a city name**. Required for tight pagination. |
Response: `localResults`; each entry has `position`, `title`, `placeId`, `dataId`, `kgmid`, `thumbnail`, `phone`, `address`, `website`, `description`, `workingHours` (object with `timezone` + `days[]`), `openState`, `rating`, `reviews`, `type` + `types[]` (categories), `price`, `priceDescription`, `gpsCoordinates`, `serviceOptions[]`, `extensions` (offerings, accessibility, payments, …), `menu`. Feed `placeId`/`dataId` into `/place` and `/reviews`.
Returns full place detail; coordinates, hours by day, phone, website, popular times, attributes (delivery, dine-in), photo summary.
YellowPages is US-only; EU/APAC searches return nothing useful.
- **`placeId` vs `dataId`**; Place prefers `placeId`; Reviews accepts either.