# tmux: Terminal Multiplexer
`tmux` keeps terminal sessions alive across SSH disconnects, splits work across multiple panes, and enables fully scriptable terminal automation. This skill covers session management, window/pane layout, keybinding patterns, and using `tmux` non-interactively from shell scripts; essential for remote servers, long-running jobs, and automated workflows.
# Prefix + c    : new window
# Prefix + ,    : rename window
# Prefix + &    : kill window
# Prefix + n/p  : next/previous window
# Prefix + 0-9  : switch to window by number
# Prefix + %    : split vertical
# Prefix + "    : split horizontal
# Prefix + arrow: navigate panes
# Prefix + z    : zoom/unzoom current pane
# Prefix + x    : kill pane
# Prefix + {/}  : swap pane with previous/next
- Always name sessions (`-s name`) in scripts; unnamed sessions are hard to target reliably
- Use `send-keys ... Enter` for automation rather than piping stdin; it works even when the target pane is running an interactive program
- `send-keys` executes commands in a pane without confirmation; verify the target (`-t session:window.pane`) before use in scripts to avoid sending keystrokes to the wrong pane
- `@bash-pro`; Writing the shell scripts that orchestrate tmux sessions
- `@bash-linux`; General Linux terminal patterns used inside tmux panes
- `@ssh`; Combining tmux with SSH for persistent remote workflows