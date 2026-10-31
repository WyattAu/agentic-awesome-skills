| `--session <id>` | Resume a specific omp session (`--session` / `--resume`); send only the delta brief. A value is required; bare `--resume` would open omp's picker and hang. |
  then sends SIGKILL if needed; on Windows there is no escalation phase; the whole process tree
work is lost; the run killed late, or the working tree damaged afterward; read the event log
reconstruction as unverified until it matches a working-tree diff; when the tree still holds the