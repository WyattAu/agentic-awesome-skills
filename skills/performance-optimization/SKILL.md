Measure before optimizing. Performance work without measurement is guessing; and guessing leads to premature optimization that adds complexity without improving what matters. Profile first, identify the actual bottleneck, fix it, measure again. Optimize only what measurements prove matters.
Two complementary approaches; use both:
// BAD: N+1; one query per task for the owner
<!-- GOOD: Hero / LCP image; art direction + resolution switching, high priority -->
<!-- GOOD: Below-the-fold image; lazy loaded + async decoding -->
// Profile before changing import styles; the real gains come from splitting and lazy loading.