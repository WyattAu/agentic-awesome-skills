| `pov.mode` | string | optional | `third-limited`, `first`, `omniscient`; documentary only |
| `timeline` | object[] | optional | `{id, date, summary, chapter}`; documentary; keeps dated events in one place |
| `aliases` | string[] | Legitimate shorter forms / nicknames; never flagged |
| `watch_spellings` | string[] | Known wrong spellings; flagged wherever they appear |
`watch_spellings` are mutually exclusive; a spelling in both is skipped, so
**warn** (the payoff may be worded differently; reword the keyword, do not