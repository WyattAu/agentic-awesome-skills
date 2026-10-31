1. **Provider failover.** Configure AI Gateway with an active-active fallback chain across providers (OpenAI + Anthropic, or model-family pairs). Critical-path agents must not be single-provider; a 429 from one provider becomes a user-visible outage otherwise. Field example: MELI runs homegrown active-active routing because retry-on-error against a single provider degraded their NLP-on-support flow.
- **Single-provider lock-in.** "We're using AI Gateway" doesn't imply failover; the provider list still has to be configured. A single-provider gateway is a thinner wrapper, not multi-provider resilience.
- **Observability Events runaway.** Captured every tool call + every streamed delta at 100% sampling; events SKU climbs above 30% of bill. Cap span cardinality before scaling traffic.
- [external-api-critical-path](../support-topics/external-api-critical-path.md); sequential vs parallel calls; AI Gateway is one external API among others
- [fluid-compute-caveats](../support-topics/fluid-compute-caveats.md); module-state hazards and shared-instance caveats
- [function-duration-io-and-after](../support-topics/function-duration-io-and-after.md); `after()` for post-response tool logging
- [observability-events-cost-attribution](../support-topics/observability-events-cost-attribution.md); when Observability Events climb above 20% of bill
- [use-cache-remote-shared-origin-data](../support-topics/use-cache-remote-shared-origin-data.md); caching shared LLM context or embedding lookups
- `https://vercel.com/docs/ai-gateway`; provider configuration, failover chain
- `https://vercel.com/docs/vercel-sandbox`; current lookup/snapshot APIs and active-compute billing