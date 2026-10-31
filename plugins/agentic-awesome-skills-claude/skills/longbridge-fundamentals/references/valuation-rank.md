Prompt-only skill. Fetches the **time series of a stock's valuation rank within its industry**; each data point shows where the stock sits among peers (e.g., rank 3 out of 28) for PE, PB, PS, and dividend yield.
# Past 1 year (default): PE / PB / PS / dividend-yield ranks
# Help: always verify flags
3. **Parse** the time series. Each data point includes a `timestamp` and sub-objects for `pe`, `pb`, `ps`, `dvd`; each containing `rank` and `total`.
**AAPL.US; Industry valuation rank history (past 1 year)**
| Other stderr                    | 直接显示原始错误，不静默重试。                            | 顯示原始錯誤。/ Surface a redacted error summary; do not retry silently.                                                            |