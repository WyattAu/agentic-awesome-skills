Transcode a video from a URL with custom resolution, quality, and audio settings. Processing happens server-side; no local ffmpeg required.
| `coll.generate_text(prompt, model_name="basic", response_type="text")` | `dict` | LLM text generation; access result via `["output"]` |
| `image.url` | `str\|None` | Image URL (may be `None` for generated images; use `generate_url()` instead) |