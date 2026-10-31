past occurrence; with the unconditional baseline so the edge is separated from the tape's drift.
| `atrPct` | 14 | ATR as % of price; volatility |
| `bbWidthPct` | 20 | band width as % of the middle; small = squeeze |
| `price` |; | close |
Periods clamp to 2–200. Conditions are AND-ed; there is no OR; run two scans for an OR and merge.
**Comparison mode**: `symbols` with no `conditions` returns every listed symbol as a table; this is
- "squeezing" → `bbWidthPct lt 4` on 4h (lower on 1h); then sort by metric ascending
The study runs over the whole stored tape; up to 1000 candles, so roughly 41 days on 1h, 166
- **Horizons that disagree** (positive at 1d, negative at 3d) usually mean a bounce that fades -
Backtest: one header line (symbol, timeframe, condition, occurrences, coverage), a horizon table -
- then the last episodes and whether the condition is active now. Close with the sample-size
  invalidate the setup, and leave the trade; and the sizing; to the user.
- Nothing in this connector trades; no order, close, transfer or withdrawal tool exists by design.