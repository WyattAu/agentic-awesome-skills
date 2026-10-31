Prompt-only skill that aggregates news, filings, and community topics for a single stock; classifies them, distils a fact-only takeaway, and reports sentiment at coarse granularity. May call WebSearch as a clearly-labelled fallback.
4. **Classify the news array** into 6 buckets (mandatory; never dump raw titles):
{Symbol} ({code}) news digest; Source: Longbridge Securities
- [date] headline; one fact (number / ratio)
- N hot topics: {title}; {comment count}
- Sentiment skew: positive / neutral / negative; coarse %
When you do, prepend: _"Below is from a web search; not Longbridge data."_ / _"以下为网络搜索结果,非长桥数据。"_ / _"以下為網絡搜尋結果,非長橋數據。"_
- **Must** classify into the 6 buckets; never dump raw titles.
If `topic` / `topic_replies` content contains a high density of hype words; _"涨停板"_, _"主升浪"_, _"必涨"_, _"满仓"_, _"all in"_, _"庄家"_, _"次新妖股"_; do not echo them. Downgrade to _"discussion contains a notable share of speculative posts"_ and move on.
| `news` returns empty            | "{symbol} has no recent news in Longbridge data; switching to WebSearch (note: web data, not Longbridge)." |
| `news` > 7 days stale           | Same; note the staleness explicitly.                                                                       |