| 400, 401, 403, 404 | Terminal client error | Do not retry; fix the request |
    // Terminal errors; do not retry
    // Retryable; but exhausted attempts
- ✅ Always respect `Retry-After` headers; they come from the provider who knows their limits
- ❌ Don't retry 4xx client errors (except 408 and 429); fix the request instead
- ❌ Don't use fixed delays; exponential backoff distributes load more evenly
- ❌ Don't retry without a cap; unbounded retries can amplify outages
- ❌ Don't ignore per-endpoint limits; some APIs have different quotas per route
- Token bucket is approximate for distributed systems; use Redis-backed rate limiting for multi-instance deployments (for example the `upstash-ratelimit` skill, or any shared-store limiter).
  **Solution:** Parse both formats; check if the value is numeric first, then try Date parsing.