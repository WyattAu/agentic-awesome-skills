| `/scrape/youtube/search` | Search results; videos, shorts, channels, playlists |
Per-video result keys (illustrative snapshot; verify current schema): `videoId`, `title`, `link`, `channel`, `description`, `length`, `views`, `viewsOriginal`, `publishedDate`, `thumbnail`, `positionOnPage`. `channel` is an object; read `.channel.name` and `.channel.link`.
| `v` | **Required.** 11-character YouTube video ID; the `v=` query value. |
`sections[]` each have `title` + `items[]`; shape depends on the tab. Iterate generically:
- **Pagination tokens are opaque**; pass them back verbatim via `paginationToken`.
- **`channelInfo.rssUrl`** is the canonical RSS feed for the channel; use it to subscribe in podcast clients without scraping.