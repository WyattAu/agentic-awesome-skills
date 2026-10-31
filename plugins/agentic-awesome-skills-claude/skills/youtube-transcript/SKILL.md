## Primary path: DeepAPI
Tell the user whenever you fall back; a fallback means the product missed a real use case.
## Fallback path: yt-dlp (local)
- **Always use `json3`, never VTT/SRT**; auto VTT repeats every line twice (rolling captions).
- **429 / "Sign in to confirm you're not a bot"** = IP flagged. STOP; do NOT retry in a loop (makes it worse).