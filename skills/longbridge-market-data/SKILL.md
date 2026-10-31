> **Response language**: match the user's input language; English / Simplified Chinese / Traditional Chinese.
### `quote`: real-time quote for one or more symbols
### `depth`: Level 2 order book (bid/ask ladder)
### `brokers`: broker queue at each price level (HK only)
### `trades`: recent tick-by-tick trades
### `intraday`: intraday minute-by-minute price and volume
### `kline`: OHLCV candlestick data or historical date-range
### `static`: static reference info (name, listing exchange, lot size, etc.)
### `calc-index`: calculated indexes (PE, PB, turnover rate, DPS rate)
### `capital`: intraday capital distribution or flow time series
### `market-temp`: market sentiment index (0–100)
### `trading`: trading session schedule and trading calendar
### `security-list`: overnight-eligible securities by market
### `participants`: market maker broker IDs and names
### `subscriptions`: active real-time WebSocket subscriptions
### `ah-premium`: A/H premium ratio for dual-listed stocks
### `trade-stats`: price distribution by volume (intraday profile)
### `market-status`: market open/close status for each exchange
### `exchange-rate`: exchange rates for all supported currencies
### `ipo`: IPO commands: calendar, subscriptions, us-subscriptions, orders, profit-loss
- `quote`, `depth`, `brokers`, `trades`, `intraday`, `kline`, `static`, `calc-index`, `capital`, `market-temp`, `trading`, `security-list`, `participants`, `ah-premium`, `trade-stats`, `market-status`, `exchange-rate`, `ipo calendar/subscriptions/us-subscriptions`: Public; no login required
| Empty result | "No data returned; verify the symbol format is `<CODE>.<MARKET>` (e.g. NVDA.US, 700.HK)" |
| Other stderr | Summarize the error after removing tokens/account identifiers; do not retry silently |