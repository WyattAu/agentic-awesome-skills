> **Response language**: match the user's input language; English / Simplified Chinese / Traditional Chinese.
Trigger when user asks about: latest news for a stock, company announcements / regulatory filings, community discussion topics, SEC EDGAR filings (10-K annual, 10-Q quarterly, 8-K material events, proxy statement) for narrative analysis (risk factors, MD&A); for structured insider trade data use `longbridge-research`, or financial regulatory rules (A-share price limits, HK T+0, US PDT rule, circuit breakers, margin requirements).
### `news`: latest news articles for a symbol; fetch full article content
### `filing`: regulatory filings list; fetch full filing content
### `topic`: community discussion topics for a symbol; keyword search