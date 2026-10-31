- Historical valuation series (PE, PB; run `--help` for available indicators and range flags)
- Optional intraday valuation correction (for live mid-day prices; run `--help` for the relevant subcommand)
3. Optional intraday correction (only when needed): run `longbridge --help` to find the subcommand for live intraday valuation; note that the standard valuation subcommand is often EOD only.
   If history is sparse (< 1y) or the industry has fewer than 5 peers, **degrade gracefully**; show snapshot + relative-to-industry only, drop the percentile claim.
{Symbol} ({code}) valuation snapshot; Source: Longbridge Securities
From historical + industry views, valuation is {low / neutral / high}; historical N-th pct, {N% above/below} industry median.
## Cyclical industries: special handling
Energy / chemicals / steel / shipping / banks / property are cyclical: PE inverts (high PE near troughs because earnings are depressed; low PE near peaks may signal a top). When the symbol is in such an industry, **add the caveat**: _"Cyclical industry; PE percentile must be interpreted alongside industry cycle position; do not read 'high PE = expensive' mechanically."_