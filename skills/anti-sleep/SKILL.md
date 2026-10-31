## Quick start: the standard command
| `-u -t 1` | simulates user activity; wakes the display right now |
**Gotcha:** `caffeinate` prints nothing and holds the prompt; it looks "stuck" or like Enter wasn't pressed. It isn't stuck. Verify with `pgrep`, not by looking at the terminal.
**Expiry:** with `-t` it exits silently when time runs out; no notification. If the user asks "is it still on?" after hours, check `pgrep` first; it may simply have expired.
`caffeinate` cannot keep the keyboard backlight on; it has its own inactivity timer with no CLI/API on Apple Silicon (researched 2026-07). Fix is manual, one-time: System Settings > Keyboard > "Turn keyboard backlight off after inactivity" > Never.