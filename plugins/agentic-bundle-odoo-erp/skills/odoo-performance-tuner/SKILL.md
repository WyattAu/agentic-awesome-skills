This skill helps diagnose and resolve Odoo performance problems; from slow page loads and database bottlenecks to worker misconfiguration and memory bloat. It covers PostgreSQL query tuning, Odoo worker settings, and built-in profiling tools.
# odoo.conf: illustrative values only; measure workload and total memory before use
limit_memory_soft = 1610612736  # 1.5 GB; worker is recycled gracefully after this
limit_memory_hard = 2147483648  # 2.0 GB; worker is killed immediately; prevents OOM crashes
  1. Click "Enable Profiling"; set a duration (e.g., 60 seconds)
- ❌ **Don't:** Set `workers = 0` in production; the non-worker server has different concurrency characteristics; use a deployment mode supported by the target OS and measured workload.
- ❌ **Don't:** Ignore `limit_memory_soft`; workers exceeding it are recycled between requests; without the limit they grow unbounded and crash.
- ❌ **Don't:** Directly manipulate `prefetch_ids` on recordsets; rely on Odoo's automatic batch prefetching, which activates by default.
- PostgreSQL tuning (`shared_buffers`, `work_mem`, `effective_cache_size`) is highly server-specific and not covered in depth here; use [PGTune](https://pgtune.leopard.in.ua/) as a starting baseline.
- **Odoo.sh** managed hosting restricts direct PostgreSQL and `odoo.conf` access; some tuning options are unavailable.