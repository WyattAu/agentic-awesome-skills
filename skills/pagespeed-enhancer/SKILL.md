description: "Scan, audit, and fix web performance issues across all four Lighthouse/PageSpeed Insights pillars; Performance, Accessibility, Best Practices, and SEO; in structured batches."
1. Parses the score summary table; identifies LCP as critical bottleneck
2. Runs Batch A scan; finds lazy-loaded hero image and render-blocking CSS
2. Runs LCP-specific audit from Batch A; checks TTFB, element render delay, lazy loading
- Does not run actual Lighthouse or PageSpeed tests; the user must provide the report or URL
- CSP guidance uses a report-only iterative approach; the final policy must be tuned to each project's actual resource origins