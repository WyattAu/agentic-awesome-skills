3. For composite questions ("give me a full picture of X as a company"); call several subcommands concurrently and merge.
**`shareholder`**; table of name / % held / change / report date. Highlight the top 3 by holding and any change > ±10% if `--range` is `all`.
**`executive`**; list of name / title / appointment date (if available). Group by role (CEO / CFO / Chair / others).
**`company`**; short profile paragraph: founding year, headquarters, employees, IPO date + price, business description.
**`corp-action`**; chronological list (most recent first): date / type (split / dividend / rights / bonus) / ratio or amount. Annotate split adjustments.
**`invest-relation`**; tree-like list: parent → company → subsidiaries (with stake % when available). Note cross-listed sister tickers.
When data is empty, state so explicitly (e.g. _"No corporate actions on record."_); do not invent.
| Other stderr                                | Relay a redacted error summary; never silently retry.                                                        |