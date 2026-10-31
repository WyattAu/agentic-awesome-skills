11. **Stop capture** when done; send SIGTERM to the capture process. It should call `client.stop_capture()` and `client.shutdown()` in its signal handler.
1. **Stop the capture session**; `client.stop_capture()` then `client.shutdown()`
2. **Wait for export event**; poll `$VIDEODB_EVENTS_DIR/videodb_events.jsonl` for `capture_session.exported`
3. **Stop the WebSocket listener**; `kill -- "$(cat "$VIDEODB_EVENTS_DIR/videodb_ws_pid")"`