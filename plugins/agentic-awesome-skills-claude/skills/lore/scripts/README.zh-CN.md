**组合而非重复。** `find_duplicates.py`、`find_stale.py` 和 `history.py` 通过 `list_entries.py --json` 复用解析器，不重复实现 entry 格式解析。Entry 格式只在一处定义--将来格式变更只需改 `list_entries.py`。
- `[WARN] .lore/.config.json has no schema_version field.` -- 配置文件存在但缺 `schema_version` 字段时，每个调用触发一次。加 `"schema_version": 1` 即可消除。
- `[WARN] .lore/.config.json#schema_version=N is newer than this lore skill expects (max: 1).` -- 配置版本超过本 skill 能理解的范围时触发。从上游 pull 最新 lore。
- `[WARN] entry <id> carries multiple #superseded-by tags; keeping the first only.` -- 一个 entry 上出现多个合法的 `#superseded-by` 标签时触发。
- `[WARN] entry <id> has a malformed #superseded-by value '<value>' (expected LAYER-YYYY-MM-DD-xxxx); chain not resolved.` -- `#superseded-by` 的值不是合法 entry ID 时触发；标签保留在文本里，`replaced_by` 保持 `None`。