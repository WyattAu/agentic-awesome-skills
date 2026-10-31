`cmux markdown open` defaults to **spawning a brand-new pane** every time, even with `--direction right`. The common "fix"; moving the new markdown surface into the existing right pane with `move-surface`; **bugs out: the moved viewer renders BLANK.** The surface keeps `type=markdown` and looks healthy, but shows nothing.
### Option A: Open it right on the first try
### Option B: Close existing right pane(s), then open fresh
# 3. THEN open the markdown fresh: it creates its own clean right pane