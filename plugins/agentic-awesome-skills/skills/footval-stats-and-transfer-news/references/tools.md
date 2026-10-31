# FootVal MCP tools: reference
## `search_football`: Search FootVal
## `find_players`: Find players
| `league` | string (≤ 60 chars) |; | League code or name, e.g. "GB1", "TR1", "Premier League", "Süper Lig" |
| `club` | string (≤ 80 chars) |; | Club id or name |
| `nation` | string (≤ 60 chars) |; | National team: ISO-3 code (TUR, ESP, BRA, ENG…) or English name |
| `position` | `GK` · `DF` · `MF` · `FW` · `CB` · `LB` · `RB` · `DM` · `CM` · `AM` · `LM` · `RM` · `LW` · `RW` · `SS` · `ST` |; | A group (GK/DF/MF/FW) or a specific position code |
| `min_age` | integer (15–45) |; |  |
| `max_age` | integer (15–45) |; |  |
| `foot` | `left` · `right` · `both` |; |  |
| `min_value` | number (≥ 0) |; | Minimum FootVal value in EUR |
| `max_value` | number (≥ 0) |; | Maximum FootVal value in EUR |
## `get_player`: Player valuation profile
## `compare_players`: Compare players
## `get_rankings`: Player rankings
| `league` | string (≤ 60 chars) |; | League code or name |
| `nation` | string (≤ 60 chars) |; | National team ISO-3 code or English name |
| `position` | `GK` · `DF` · `MF` · `FW` |; |  |
| `max_age` | integer (15–45) |; |  |
## `list_leagues`: List leagues
## `get_league`: League overview
## `get_club`: Club squad & value
## `get_national_team`: National team squad
The strongest squad FootVal can assemble for a national team (3 GK, 8 DF, 8 MF, 7 FW; top players by value among those who represent the nation), with squad value and average age. Accepts an ISO-3 code or an English country name.
## `get_matches`: Scores & fixtures
| `competition` | string (≤ 60 chars) |; | Competition code or name, e.g. "GB1", "UCL", "Süper Lig" |
| `team` | string (≤ 60 chars) |; | Only matches involving this team (part of its name) |
## `get_match`: Match centre
## `get_team_fixtures`: Team fixtures
| `national` | boolean |; | true = national team, false = club |
## `get_standings`: League table
| `season` | integer (2010–2100) |; | Season start year, e.g. 2025 for 2025/26 (default: current) |
## `get_stat_leaders`: Statistics leaders
Season leaderboards; top scorers, assist leaders, goals + assists, minutes or appearances; for one competition (a league, the Champions League, a domestic cup), all national-team competitions ("NT"), or every competition combined ("all"). Includes players outside FootVal's tracked leagues.
| `season` | integer (2010–2100) |; | Season start year (2025 = 2025/26). Default: the latest season with data |
| `position` | `GK` · `DF` · `MF` · `FW` |; |  |
## `get_transfer_news`: Transfer news
| `query` | string (≤ 60 chars) |; | Player or club name to filter by |