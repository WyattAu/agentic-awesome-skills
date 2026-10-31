Use this reference when the user asks to record a new trace; either to
When you (the agent) are running non-interactively; e.g. via
`Bash run_in_background`; use a stop-file so you can signal the
# List every connected device, simulator, and the host: JSON.
# List all Instruments templates: JSON with a flat list + by-section map.
`name`, `os`, `udid`. Offline devices are known but unplugged / unpaired -
- **Device offline**; `--list-devices` shows it in `devices offline`.
- **Output path exists**; the script refuses to overwrite. Either pick
- **App not running (for `--attach`)**; xctrace exits with an error;
- **Signing / trust on device**; iOS requires a development build