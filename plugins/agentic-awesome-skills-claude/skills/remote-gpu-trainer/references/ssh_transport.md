# SSH Transport: keys, keepalive, resumable copy, secrets-via-stdin
hosts, ports, and credential locations are **profile facts**; this file owns the *mechanism*, the
5. Resumable copy; rsync vs scp, and WHY rsync
7. Move secrets via stdin; never inline a key, never on a durable FS
8. CRLF; `.sh` authored on Windows breaks on Linux
9. Two SSH flavors; proxied/basic SSH cannot `scp`
reused across all instances; generate once, push the **public** half (§2) to each box. The private
a shared FS, or a cloud agent (a cloud scheduler runs in an isolated sandbox with no access to it; and
- `ServerAliveInterval 60`; send an application-layer heartbeat every 60 s, so a NAT/idle timeout on
- `ServerAliveCountMax 120`; tolerate up to 120 missed heartbeats before declaring the link dead (≈2 h
  blip rather than hang; see the short-connection poll in `references/monitoring_patterns.md`.
- `TCPKeepAlive yes`; let the OS also emit TCP-layer keepalives, catching a peer that vanishes
re-creation); update the `Port` line after each create/recreate, then re-run §4.
## 5. Resumable copy: rsync vs scp, and WHY rsync
a re-run after a drop **continues** instead of restarting; the single most important property on a
- Re-run the **identical** command after any failure; that *is* the resume (principle #7).
> *download-from-the-internet* concern, not host↔host copy; that lives in `references/china-network.md`.
- **List once, copy per-dir**; each `rsync --partial --protect-args` is an independent session.
- **Reconcile every directory**; rsync compares source and destination on each run; a size threshold
## 7. Move secrets via stdin: never inline a key, never on a durable FS
*block or corrupt* a file matching a known key pattern; so a credential written to the cross-instance
# stream exactly one credential block: value never appears on a command line
- **One block, not the whole file.** Stream a single `machine …` stanza, never the entire `~/.netrc` -
## 8. CRLF: `.sh` authored on Windows breaks on Linux
  option`, `cd: /path\r: No such file or directory`, or `syntax error near unexpected token $'do\r'` -
  unaffected; Python's universal newlines tolerate CRLF; specifically `bash`/`.sh` breaks.)
poll script; the shell splits it into piped commands and the first reads stdin → hangs forever
## 9. Two SSH flavors: proxied/basic SSH cannot `scp`
- **Direct TCP SSH**; a real TCP port to the container (the `connect.<region>.<provider>.com:<PORT>`
- **Proxied / "basic" SSH**; a relayed or web-terminal SSH (common on RunPod and vast.ai for the
**Fix**: for any code/data/checkpoint transfer, use the **direct-TCP** endpoint; on RunPod expose a
Universal gotchas (disk-full, inode, OOM, silent sync) are **not** repeated here; see
**T1; SSH exits 255 / "Connection reset" right after a `pkill`/`kill`.**
cause: killing the process tree disrupts the PTY chain; the SSH client receives EOF and exits; and
into **two** ssh calls; never `pkill X; relaunch X` in one command, the relaunch is dropped with the
**T2; large `scp -r` drops with "Read from remote host … reset by peer" 30–60 min in.**
→ Fix: use `rsync --partial` (§5) or the per-dir loop (§6); each dir an independent session, re-run
**T3; `.sh` "ends in `\r`" after a Windows→Linux sync.**
**T4; a credential leaks into history / a shared FS, or its FS upload silently fails.**
command line; and some platforms' FS classifiers block/corrupt credential-shaped uploads. → Fix: §7 -
**T5; `scp dest open "/root/x/": Failure` instantly.**
destination dir doesn't exist; a sibling command meant to `mkdir` it ran later, or was blocked. → Root
race). → Fix: make every transfer self-sufficient; create the dest in the same command:
**T6; `Host key verification failed` after an instance is recreated.**