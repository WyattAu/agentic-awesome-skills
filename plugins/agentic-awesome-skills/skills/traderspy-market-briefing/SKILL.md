volume of data; every tool call costs the user quota, so the playbook below is deliberately
1. **Majors**; `get_price` with `["BTCUSDT","ETHUSDT","SOLUSDT"]` plus any coins the user has
2. **Positioning**; `get_derivatives` with `["BTC","ETH","SOL"]` (add up to two of the user's
3. **Movers**; `screen_symbols` with no conditions, `sortBy: "change24h"`, `limit: 50`, `universe:
4. **What is stretched**; `screen_symbols` with `[{"metric":"rsi","op":"lt","value":30}]` on 4h,
   `limit: 5`. If it returns nothing, flip to `rsi gt 70`; one of the two usually has names.
5. **Signal engine**; `get_signal_stats` with `period: "24h"` (or `7d` for a weekly recap).
6. **Freshest ideas**; `get_signals` with `limit: 5`.
rather than dropping it silently; a briefing with a hole is more honest than one that quietly
  crowded long is a risk, not a signal); negative means shorts pay. Quote the annualized figure -
- **Movers** are the most-traded pairs by 24h volume sorted by change; a liquid-names list, not
ALWAYS use this structure, in this order, with real numbers from the calls; no placeholders:
**Crypto brief; <date, UTC time of the calls>**
1. **Headline**; one sentence: the majors' direction and the single most notable positioning fact.
2. **Majors**; a table: Symbol · Price · 24h % · 24h range.
3. **Positioning**; per major, one line: funding (label, annualized), OI regime, top-trader long %,
4. **Movers**; top 5 gainers and top 5 losers among the most-traded pairs (symbol, price, 24h %).
5. **Stretched**; the oversold or overbought names with their RSI, or "none on the 4h" if empty.
6. **Signal engine**; `total` signals in the window, hit rate with the resolved count, then the
7. **What to watch**; two or three bullets derived from the data above (a funding extreme, an OI
  facts and what would confirm or invalidate them; never "buy the dip" or "short this".
- Nothing in this connector trades; no order, close, transfer or withdrawal tool exists, by