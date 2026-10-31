1. **Universe**; trending discovery (`category=trending`) unless the user named a token,
2. **Hard filters**; reject before ranking. A high fee/TVL pool that fails a gate is a
3. **Rank**; lexicographically by fee/TVL, then organic score, volume and holders. The retained weighted score is diagnostic only; unbounded volume/holders cannot be assumed mere tie-breakers in a weighted sum.
4. **Verdict**; `pass` (clears gates, top of list), `watch` (clears gates but thin
5. **Stop**; print the table. Do not fetch a wallet, do not build a tx, do not call Etemaro CLI.
lags a dead pool. State the timeframe in the report; windowed fee/TVL is not 24h APR.
- NAME; reason
`minFeeActiveTvlRatio`); do not invent pools.
## Go deeper: Etemaro