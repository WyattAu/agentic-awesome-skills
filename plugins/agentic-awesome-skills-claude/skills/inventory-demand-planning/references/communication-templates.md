# Communication Templates: Inventory Demand Planning
> **Reference Type:** Tier 3; Load on demand when composing or reviewing demand planning communications.
- Do not include forecast data or inventory levels in routine POs; this is proprietary information.
**Subject:** `PO {{po_number}}; {{vendor_name}}; Delivery {{delivery_date}}`
Firm and data-driven. You are not complaining; you are presenting evidence and requesting a corrective action plan. Lead with the impact to your business, not the vendor's failure. Offer collaboration: you want to solve this together, but you need a commitment.
- Do not speculate on the cause of the lead time issue; let the vendor explain.
- Do not use vague language like "often late"; provide specific PO numbers, dates, and deviations.
**Subject:** `Lead Time Performance Review; {{vendor_name}}; Action Required by {{deadline_date}}`
Urgent, concise, action-oriented. The audience is internal (planning manager, category merchant, supply chain director). Lead with the impact, follow with the facts, close with the recommended action. This is not a post-mortem; it's a call to action.
- Do not present multiple options without a recommendation; decision-makers need a clear ask.
**Subject:** `🔴 STOCKOUT ALERT; {{sku_description}}; {{store_count}} locations at risk`
**Item:** {{sku}}; {{sku_description}}
1. **Immediate:** {{action_1}} (e.g., "Reallocate 400 units from low-velocity stores to stockout locations; list attached")
2. **Short-term:** {{action_2}} (e.g., "Expedite PO {{po_number}}; vendor confirmed can ship 800 units by {{expedite_date}} at ${{expedite_cost}} additional freight")
3. **If above fails:** {{action_3}} (e.g., "Substitute with {{alt_sku}}; similar product, available in DC, can ship to affected stores within 48 hours")
Data-driven and collaborative. You are presenting a financial analysis, not demanding a price change. Merchandising owns pricing decisions; your job is to provide the inventory data and margin impact analysis to inform their decision. Frame recommendations as margin recovery, not "we bought too much."
- Do not propose a specific retail price; propose a markdown depth (% off) and let merchandising set the price.
**Subject:** `Markdown Recommendation; {{sku_description}}; {{excess_units}} units excess`
- Do not present a single point estimate without a confidence range; this gives false precision.
- Do not omit the post-promo dip; it's as important as the lift.
**Subject:** `Promotional Forecast; {{sku_description}}; {{promo_start}} to {{promo_end}}`
| SKU | {{sku}}; {{sku_description}} |
| Promotion Period | {{promo_start}}; {{promo_end}} ({{promo_weeks}} weeks) |
| Pre-promo (week before) | {{baseline}} units |; | {{baseline}} units |; |
| Recovery (Week 3+) | {{baseline}} |; | {{baseline}} units |; |
**Subject:** `Safety Stock Adjustment; {{sku_description}}; {{adjustment_direction}} by {{adjustment_pct}}%`
**Item:** {{sku}}; {{sku_description}} ({{abc_class}})
{{deadline}}; needed before the next replenishment cycle to take effect.
Transparent and falsifiable. The purpose of this document is to make every assumption explicit so that the post-mortem can identify where the forecast diverged from reality. Do not hedge with vague language; state the assumptions clearly so they can be validated or disproved.
**Subject:** `New Product Forecast Assumptions; {{sku_description}}; Launch {{launch_date}}`
| SKU | {{sku}}; {{sku_description}} |
Pragmatic and action-oriented. The liquidation plan is an acknowledgment that margin recovery is limited and the priority has shifted to cash recovery and warehouse space liberation. Present the options dispassionately; the goal is to make the best of a bad situation, not to relitigate the buying decision.
**Subject:** `Excess Inventory Liquidation Plan; {{sku_description}}; {{excess_units}} units`
| SKU | {{sku}}; {{sku_description}} |
{{recommendation_rationale}} (e.g., "Option B (liquidation) recovers $3,200 compared to Option A's $4,100; but Option A requires 8 more weeks of shelf space that has a higher-value alternative use. The opportunity cost of holding the shelf space exceeds the $900 margin difference.")