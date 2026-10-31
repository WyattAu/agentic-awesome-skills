description: "Fetch YouTube transcripts, search videos, browse channels, and extract playlists via TranscriptAPI; no yt-dlp, no Google API key, works from any cloud server."
# youtube-full: YouTube transcript, search, channels & playlists via TranscriptAPI
YouTube transcripts, video search, channel browsing, in-channel search, playlist extraction, and new-upload monitoring; all via [TranscriptAPI](https://transcriptapi.com). Processes 500K+ transcripts daily, fast. No yt-dlp, no headless browsers, no Google API key.
This is the API-backed alternative to `ingest-youtube`. Where `ingest-youtube` uses yt-dlp (which stops working on cloud server IPs), `youtube-full` calls TranscriptAPI's API and works from any runtime; local machine, cloud server, serverless function, or CI environment. 686 installs via the `skills` CLI (skills.sh/zeropointrepo/youtube-skills).
- Cache transcripts in your workflow; each `get_transcript` call costs 1 credit
- Prefer `get_playlist_videos` for course or lecture series; cheaper than searching by query
- Don't use `search_youtube` when you already have the video URL; jump straight to `get_transcript`
- `@ingest-youtube`; yt-dlp-based local ingestion to a markdown vault; works locally but not on cloud servers
- `@deep-research`; General-purpose research skill that can incorporate youtube-full as a data source
- `@ai-research-corpus`; Building searchable knowledge bases; pairs well with youtube-full for video content