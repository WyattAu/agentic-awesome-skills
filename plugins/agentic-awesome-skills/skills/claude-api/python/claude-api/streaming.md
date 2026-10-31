# Streaming: Python
1. **Always flush output**; Use `flush=True` to show tokens immediately
2. **Handle partial responses**; If the stream is interrupted, you may have incomplete content
3. **Track token usage**; The `message_delta` event contains usage information
4. **Use timeouts**; Set appropriate timeouts for your application
5. **Default to streaming**; Use `.get_final_message()` to get the complete response even when streaming, giving you timeout protection without needing to handle individual events