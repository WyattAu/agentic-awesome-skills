Generates a concise equity research snapshot for a single stock by aggregating analyst consensus, key financials, valuation, 60-day price performance, and recent news; structured like a sell-side research brief.
`<CODE>.<MARKET>`; e.g. `NVDA.US`, `700.HK`, `600519.SH`, `300750.SZ`, `D05.SG`. If the market is ambiguous, ask the user.
# Income statement (revenue, net income, EPS: quarterly)
1. **Company snapshot**; name, market, sector, market cap, current price + 60-day change
2. **Financial highlights**; revenue / net income / EPS (last reported + YoY growth)
3. **Valuation**; PE / PB / PS vs industry median (if available)
4. **Analyst consensus**; avg target price, buy/hold/sell count, implied upside
5. **Price performance**; 60-day return, high/low range
6. **Key news**; top 3–5 items with date and one-line summary
7. **Investment considerations**; balanced bull/bear summary (2–3 points each)