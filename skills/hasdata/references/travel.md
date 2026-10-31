# Travel APIs: Airbnb, Booking, Google Flights
| `childrenAgesJson` | Required iff `children > 0`; JSON array of ages (0–17), one per child. |
| `price[min]` / `price[max]` | `>= 10` / `>= 20`. Bracketed; pass explicit bracketed keys in Python `requests`; verify Axios serialization. |
| `returnDate` | Optional; omit for one-way. |
- **Airbnb requires `checkIn`** and uses **token** pagination; store `nextPageToken`, not page numbers.
- **Booking `price[min]` / `price[max]`** are bracketed; use explicit bracketed keys with Python `requests`.
- **Booking `rooms[i].variants[]` is where prices live**; the parent `rooms[i]` describes the floor-plan, variants are the buyable rates with `priceBreakdown` / `cancellationPolicy` / `mealPlan`.
- **`bookingDetails` carries the resolved stay context** the response was priced for; echo it back when persisting results so future comparisons use the same dates / occupancy.
- **Round-trip vs one-way** is determined by `returnDate` presence; pass it for round-trip, omit for one-way.