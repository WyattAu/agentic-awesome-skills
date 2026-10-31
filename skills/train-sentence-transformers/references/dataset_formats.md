- `"triplet"`; `(anchor, positive, negative)` triplets. One row per `(query, negative)` pair.
- `"n-tuple"`; `(anchor, positive, negative_1, negative_2, ..., negative_N)` (1-indexed); one row per query.
- `"labeled-pair"`; `(anchor, text, label)` with `label=1` for positives and `label=0` for negatives. Good for `BinaryCrossEntropyLoss`.
- `"labeled-list"`; `(anchor, texts, labels)`; one row per query with a list of candidates. Good for listwise losses.
`scripts/mine_hard_negatives.py` is a CLI wrapper; see it for a ready-to-run command.
- `range_min=0`, `range_max=100`; sample from the top-100 retrieved. Good default.
- `range_min=10`, `range_max=100`; skip the top-10 (often contains true positives). Safer if you lack a cross-encoder.
- `range_min=0`, `range_max=1000`; wider net, more diverse negatives, slower.
- `sampling_strategy="top"`; always pick the rank-1 hardest. Maximum training signal per row.
- `sampling_strategy="random"`; pick randomly within the range. More robust if your retriever is itself noisy.
`hf datasets sql "SELECT * FROM 'hf://datasets/<id>/<split>' LIMIT 5"` streams rows via DuckDB without `load_dataset(...)`; the fastest way to confirm column names match your loss before a full validation run. `hf datasets info <id>` shows config / splits / size; `hf datasets card <id> --text` renders the README.
- **Mined hard negatives with `include_positives=True`** include the positive as a negative in the output list; only useful when you're building an evaluator or want to measure rank of the positive. For training, leave it `False`.