# Running your app on the remote sim: tested sequences
The remote sim boots blank. You install a **simulator-targeted** build onto the session, then open it. Pick a mode from `SKILL.md`. (Sequences validated against eas-cli 20.3.x + agent-device 0.17.x in mid-2026. These commands are experimental; check the relevant subcommand's `--help` before using non-default flags.)
# Describe what the run is for, in the user's terms: see "Always name the session" in SKILL.md.
  echo "$S" | grep -qE '"status": *"(STOPPED|ERRORED)"' && { echo "boot failed; stop + restart"; break; }
If you need the id explicitly, it's `EAS_SIMULATOR_SESSION_ID` in `.env.eas-simulator`. `start` also prints a `webPreviewUrl` (browser preview when supported by this session/platform; surface it per the SKILL.md "watch it live" rules) and a job-run URL. If all poll attempts expire, report that readiness was not confirmed and stop the owned session rather than continuing. HTTP/CLI failure is not proof of readiness. Once live, the session env is in `.env.eas-simulator`, so `simulator:exec` works.
## Targeting a device: iPad, or several at once
**Boot a specific device at session start** with `eas simulator:start --device "<name|UDID>"` (eas-cli ≥ 22.4.0); this is how you run on an iPad instead of the default iPhone:
# then install / launch / screenshot as usual: the iPad renders larger (e.g. 1032x1376).
- ⚠️ The **controller** `--device` resolves by **NAME only**; a udid returns `DEVICE_NOT_FOUND`. (The start-time CLI `--device` above takes either.)
## Mode A: Local release build (embedded JS, no Metro)
# pod install can fail on Ruby 4 + CocoaPods with a Unicode/ASCII-8BIT error: fix with a UTF-8 locale:
## Mode B: EAS build (the VM downloads it; no credentials)
⚠️ **Check for an existing build first.** Before triggering a new build, check if a fingerprint-matched one already exists; it saves ~15-20 min:
⚠️ **Order matters:** build FIRST, `start` the session LAST. The build takes ~15-20 min and a session left idle that long times out (`ERR_NGROK_3200`); don't `start` until you have the artifact URL.
#    If not, add one named "sim": use node, python3, jq, or a direct JSON edit, whichever
## Mode C: Dev build + tunnel (live edits via Fast Refresh)
- **Method 1 (recommended, eas-cli ≥ 22.4.0):** launch at session start; `simulator:start` installs the build, applies launch-args, and opens the Metro URL in one command, and the "Open in?" dialog is auto-handled. Needs a **remote** build source (`--build-id`, `--application-archive-url`, or `--expo-go`); a local `.app` can't be passed here.
- **Method 2 (fallback):** drive the connect with the controller; for a **local `.app`** build, or eas-cli < 22.4.0.
⚠️ **Don't install a release build as a "quick interim" and screenshot it**; it shows stale, build-time code. Use a dev build + Metro; screenshot only after the dev client is connected.
### Method 1: launch at session start (recommended)
⚠️ **Metro FIRST, then `simulator:start`**; the runner opens `--open-url` during startup, with no retry.
#    Capture the manifest host. Headless runs won't print it: read ngrok's API (curl -s 127.0.0.1:4040/api/tunnels)
#    persists session-wide: so NO `alert accept` is needed, here or for later controller opens.
#    `start` prints NOTHING about the install/launch: confirm from Metro's `iOS Bundled …` line.
# 3. To screenshot/drive, ATTACH the controller once: the CLI launch makes NO agent-device session, so a bare
### Method 2: drive the connect with the controller (fallback)
# "Open in '<app>'?" dialog: accept it (no-op if absent; not needed after a Method-1 launch):
- `-EXDevMenuIsOnboardingFinished 1`; skip the first-run onboarding popup (dev client **and** Expo Go)
- `-EXDevMenuShowsAtLaunch 0`; don't auto-open the dev menu at launch (dev client)
- `-EXDevMenuShowFloatingActionButton 0`; hide the floating gear (defaults visible on both)
Start Metro on your OWN free port; each run gets its own tunnel URL, so never fight for or kill :8081 (#133's rule). BOTH backends accept ANY `--port`:
- **ws-tunnel v2 (account-signed):** `EXPO_UNSTABLE_TUNNEL_V2=1`; signed URL for your EAS account, `on.expo.app` host, and the path for robot/EXPO_TOKEN/cloud agents (plain ngrok is blocked for them). Needs login / an EAS-linked project; if the signed URL fails, the CLI says to unset the flag and use ngrok.
The ONLY 8081 lock is the LEGACY ws-tunnel path; hit WITHOUT the v2 account URL (an older CLI where the flag no-ops, or `EXPO_FORCE_WEBCONTAINER_ENV=1`). Do NOT set `EXPO_FORCE_WEBCONTAINER_ENV` to "fix" a port; it forces that legacy path and locks you to 8081. On an older CLI (e.g. expo 56) the v2 flag no-ops and you get ngrok on your chosen port (verified: expo 56.0.3 → ngrok on :8083).