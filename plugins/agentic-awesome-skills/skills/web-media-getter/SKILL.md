Use when a task needs a REAL or ARCHIVAL photo / clip (hero, texture, reference, historical footage) or a reaction / animated GIF, rather than a generated one; fan out across free image/video/GIF sources in one query and download license-tagged results.
in `central/.env` (optional; the 5 no-key sources work without them).
| klipy | free key | **GIFs**; recommended (free, unlimited, Tenor drop-in) | gif |
| giphy | free key | **GIFs**; biggest library (prod key needs approval) | gif |
in `central/.env`. (tenor adapter removed; Google EOL'd the API 2026-06-30.)
its adapter is **unverified; assumes Tenor-compatible** request/response;
`{source, title, url, thumb, dl, page_url, author, license, w, h, type}` -
  - **Twelve Labs** Marengo search (free 600 min); pass the IA public MP4 URL, get a
- **`freesound-fetch.py "<query>" [count] [max_sec] [out_dir]`**; searches freesound.org
  (token-based read; full originals would need OAuth; previews suffice for SFX).
- **`audio-judge.py <file> "<target>"`**; sends the clip to OpenAI `gpt-audio`
  like "grating"; it labels nearly any beep "sharp/high-pitched". Use it to cull, not
This is the **internet-retrieval** capability; peer to `muser` (local semantic search)