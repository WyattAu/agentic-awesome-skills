## Step 4: Score and report
- [Impact framing; the magnitude rule](#impact-framing--the-magnitude-rule)
## Impact framing: the magnitude rule
Performance numbers come from `signals.json.metrics.*`; they're observed, not extrapolated. Cite the exact route + metric value.
  fractionReduced?: number;    // 0.2 = ~20%; internal only, NOT rendered
When a rec spans buckets; e.g., a caching fix that reduces both cost AND latency; render both lines.
# Vercel Optimization Report: {projectName}
Total billed: {usage.totals.billedCost} (we render the precise current cost; we just don't project future precise savings)
If `vercel usage` was queried and unavailable, the cost breakdown is replaced by an observability-derived cost ranking from `metrics.fnGbHrByRoute` + `metrics.fnCpuMsByRoute` + `metrics.fdtByRoute` when those metrics exist. These don't translate directly to dollars; they show *which routes consume the billable units* so the user knows what to attack first. If `usageError` is `NOT_COLLECTED_OBSERVABILITY_BLOCKED` or another `NOT_COLLECTED_*` value, say usage was not collected; do not describe it as a billing-plan or Costs-feature finding.
- `Observability Plus enabled; per-route metrics included` when `observabilityPlusUsable=true`.
- `Per-route metrics unavailable; audit paused before metric-backed route ranking` when an Observability Plus blocker stopped the run before billing/scanner collection.
- `Per-route metrics unavailable; analysis based on billing + scanner findings` when the user accepted a limited audit and billing/scanner signals were collected.
- `Per-route metrics unavailable; limited analysis based on scanner findings` when the user accepted a limited audit but billing usage was queried and unavailable.
- `Not checked; audit paused at unsupported-framework preflight` when framework support stopped the run before the Observability Plus check.
- `Not enabled; analysis based on billing + scanner findings` only for legacy/limited reports where Observability Plus is known false and billing/scanner signals exist.
1. **{route or file}**; {o11ySignal}
1. **{readable candidate label}**; {readable metric labels}
(what the project is doing right; caching is healthy on routes X/Y/Z; Fluid Compute is enabled; etc.)
(what we couldn't measure; Observability Plus disabled means no per-route latency, etc.)
`ai-application` is checked first in `inferPlaybook()`; an AI-heavy SaaS or AI commerce app shares the AI playbook's billing shape (AI Gateway dominant) and gotchas, not the dashboard or cart-checkout patterns.
Playbooks shape phrasing and ordering of recommendations. They never invent claims; every rec must still trace back to verified findings.