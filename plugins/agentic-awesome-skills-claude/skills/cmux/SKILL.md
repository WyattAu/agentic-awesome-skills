- **Window**; top-level macOS cmux window
- **Workspace**; sidebar tab within a window (one git branch / project context)
- **Pane**; split region inside a workspace
- **Surface**; tab inside a pane (terminal or browser)
### Ref syntax: get this right or fail silently
- **Always use PREFIXED refs** (`pane:38`, `surface:46`). A **bare number is treated as an INDEX, not an ID**; `--surface 46` means "the surface at index 46" (usually nonexistent → silent failure), NOT `surface:46`.
- **`read-screen` and `capture-pane` have NO `--pane` flag**; they target `--workspace` or `--surface` only. Passing `--pane` errors, and a bare/missing target falls back to your OWN surface (you'll read your own footer and draw wrong conclusions). To read a pane: resolve it to a surface FIRST with `cmux list-pane-surfaces --pane pane:N`, then `cmux read-screen --surface surface:N`.
Injected env vars in every cmux-spawned terminal: `CMUX_WORKSPACE_ID`, `CMUX_SURFACE_ID`, `CMUX_SOCKET_PATH`, `CMUX_PORT`. **Always anchor automation to `CMUX_WORKSPACE_ID`**; the visually focused workspace may not be the agent's caller workspace.
## Fast Start: Topology
## Polling Pi Agents in Panes: Keep Sleeps Short
`cmux markdown open` flags: `--workspace`, `--surface`, `--window`, `--direction <right|down|left|up>`, `--focus <true|false>`. There is **NO `--pane` flag**; passing it errors. To target a pane, pass `--surface <existing-md-surface-in-that-pane>`.
To replace the doc shown in your one right markdown pane, an upstream workaround is **close the previous surface FIRST, then `markdown open` the new file fresh**; never move an existing viewer, never open-then-close.
- **Surface refs are global, not per-workspace.** A ref like `surface:126` from an earlier `markdown open` may live in a different window/workspace. Always re-list (`list-panes` / `list-pane-surfaces`) before reusing a ref; never assume a ref from a previous turn is still in the right pane.
- **`move-surface`-ing a markdown viewer often leaves it BLANK.** The moved surface keeps `type=markdown` and `surface-health` looks fine, but renders nothing. Fix: `close-surface` it and `cmux markdown open <path>` fresh, then move the *fresh* surface if needed. Don't waste time on `refresh-surfaces`; it usually won't fix a moved-then-blank viewer.
cmux docs settings        # prints paths, schema URL, reload cmd; read BEFORE editing
- Terminal rendering (font, cursor, theme, scrollback, opacity, blur): `~/.config/ghostty/config`; NOT cmux.json.
`/tmp/cmux.sock`; Unix socket, JSON-RPC v2. Use for tight loops where subprocess spawn cost matters; otherwise prefer the CLI.
Access modes: `cmuxOnly` (default; only cmux-spawned processes), `automation` (any local process), `password`, `allowAll` (unsafe). If you hit `Failed to connect to socket`, you're likely an external process under `cmuxOnly`; run from an authorized cmux terminal; broadening socket access needs explicit authorization and least-privilege configuration.
## Critical Rules: Non-Disruptive Automation
- **Don't `cat ~/.cmuxterm/*-hook-sessions.json`** expecting secrets; they're scrubbed. Look there for session/surface mappings only.