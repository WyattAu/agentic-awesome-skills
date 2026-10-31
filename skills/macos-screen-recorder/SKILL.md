description: "macOS screen recorder that captures the main display PLUS system audio via ScreenCaptureKit; no BlackHole/loopback driver, no sudo, just the standard Screen Recording permission. CLI-driven; fills the headless-screen-recording-with-system-sound gap QuickTime and `screencapture -v` can't."
Use when you need to script a screen recording WITH system sound on macOS from the CLI (demos, captures, voice-demo recording); the case QuickTime and `screencapture -v` can't cover without a virtual audio device.
**Intended use:** system audio from the CLI after a reviewed local build; no
Studio" tool; it fills exactly the headless-CLI-with-system-audio gap.
`sck-record` is the raw capture primitive; it records, nothing more. To polish a