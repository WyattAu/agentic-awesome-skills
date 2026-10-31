1. **Identify legs**; extract each leg: side (buy/sell), type (call/put), strike, expiry, quantity, premium paid/received.
2. **Fetch live data**; call `longbridge option quote` for each OCC symbol; if not known, use `option chain` to find it. Fetch underlying spot with `longbridge quote`.
4. **Find breakeven(s)**; stock price(s) where net payoff = 0.
5. **Find max profit and max loss**; evaluate same-expiry piecewise-linear segments at strikes and S=0, then inspect the high-price tail slope for unbounded outcomes. A finite scan cannot prove a finite maximum loss.
{Strategy name} P&L snapshot; Source: Longbridge Securities