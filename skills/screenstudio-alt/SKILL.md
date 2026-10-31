description: "Open-source headless Screen Studio alternative: auto speed-up of idle, auto-zoom on click clusters, keystroke overlay chips, smoothed synthetic cursor, and 9:16 vertical export that follows the action; post-production for screen recordings from the CLI."
- `events-log` (Swift); capture-side input logger (cursor 60Hz, clicks, keys;
  this data at capture time; it cannot be recovered from pixels later.**
- `polish.py` (Python, ffmpeg + PIL); the post-production pass:
`--speedup` works WITHOUT events (freezedetect only); usable on the whole
- `render.py`; **high-quality non-destructive renderer** (preferred): single-pass
- `studio.py [recording.mp4]`; local web UI, **NLE-style fixed-ruler timeline** (bar =
  **speed blocks with rate-only editing**; source range locked, rate set via inspector
  per-frame exprs). That's why zoom uses `zoompan` (no `t` var there; use `on/FPS`).
  events.jsonl; validate any change against it before trusting real footage.