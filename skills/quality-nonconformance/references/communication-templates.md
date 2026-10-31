# Communication Templates: Quality & Non-Conformance Management
> **Reference Type:** Tier 3; Load on demand when composing or reviewing quality communications.
| `{{part_description}}` | Part description | `Shaft, Output; Titanium` |
| `{{severity_level}}` | NCR severity classification | `Major; Dimensional non-conformance on key characteristic` |
Factual and direct. Internal teams need to know what happened, what the scope is, and what the immediate impact is. No blame, no speculation; data only. Include enough detail for engineering to begin their assessment and for planning to evaluate the production impact.
**Subject:** `{{ncr_number}}: {{part_number}}; {{defect_description}}`
**Part Number:** {{part_number}}; {{part_description}}
Formal, precise, and complete. This is a controlled document. Every required field must have verified information or an explicit pending/not-applicable status with rationale. Engineering justification must be technically sound and specific; not "acceptable per engineering review" but a detailed rationale citing functional requirements.
{{risk_assessment_reference}} (e.g., "Per ISO 14971 risk assessment RA-2025-0012, risk level is acceptable; severity [minor], probability [remote]")
☐ Not required; documented applicability rationale: {{approval_not_required_rationale}} ☐ Requested; Reference: {{customer_approval_ref}} ☐ Approved; Date: {{approval_date}} ☐ Denied
**CAPA Required:** ☐ Yes; {{capa_number}} ☐ No; Rationale: {{no_capa_rationale}}
Professional, specific, and structured. Provide all data the supplier needs to investigate. Set clear expectations for the response format and timeline. Do not be accusatory; present the facts and ask for investigation. The supplier's willingness and quality of response will tell you whether this is a fixable issue or a systemic problem.
- Do not speculate on the root cause; that's the supplier's job
**Subject:** `SCAR-{{scar_number}}: Non-Conformance on PO# {{po_number}}; Response Required by {{response_deadline}}`
- Part Number: {{part_number}}; {{part_description}}
1. **Containment actions**; immediate actions to protect our inventory and any other customers who may have received material from the same lot. Confirm whether other lots from the same production run may be affected.
2. **Root cause analysis**; we require a rigorous root cause investigation, not a surface-level explanation. "Operator error" or "inspection escape" are not acceptable root causes. Identify the systemic process or system failure that allowed this non-conformance.
3. **Corrective actions**; specific, measurable actions addressing the verified root cause. Include implementation dates and responsible personnel.
4. **Effectiveness verification plan**; how and when will you verify that the corrective actions are effective?
☐ Return to Vendor; please issue RMA# and shipping instructions
☐ Sort at our facility; proposed sort cost allocation requires contractual review and approval
☐ Scrap at our facility; proposed material credit requires contractual review and approval
Example: "Recurring dimensional non-conformance on Part 7832-A Rev D; bore diameter out of tolerance (>USL of 12.50mm). Three NCRs in the last 60 days (NCR-2025-0398, -0412, -0456) affecting lots from three different production runs. Total scrap cost to date: $14,200. No customer impact confirmed, but risk of escape exists based on inspection sampling rates."
Data-driven and conclusive. The effectiveness review is where the CAPA either closes with evidence of success or reopens with evidence of failure. Auditors specifically review effectiveness evidence; it must be quantitative and linked to the original problem statement.
☐ **CAPA Effective; Recommend authorized closure.** Approved effectiveness criteria met with adequate exposure and stated uncertainty; implementation and adverse-effect checks documented.
☐ **CAPA Partially Effective; Extend monitoring.** Improvement demonstrated but monitoring period insufficient for definitive conclusion. Extend by {{extension_days}} days.
☐ **CAPA Not Effective; Recommend controlled reassessment/reopening or linked investigation.** Recurrence observed during monitoring period. Root cause re-investigation required. See {{reopened_investigation_ref}}.
- Proactive notification; the customer should hear about it from you before they discover it themselves
Transparent, action-oriented, and structured. Lead with what you know and what you've done (containment), not with excuses. Provide the specific traceability data the customer needs to identify and segregate affected product in their inventory. The customer will judge your quality system based on how you handle this notification; transparency and speed build trust; delay and vagueness destroy it.
**Subject:** `Quality Notification: {{part_number}}; {{defect_description}}; Action Required`
- Part Number: {{part_number}}; {{part_description}}
Urgent, clear, and actionable. This goes to the production floor; operators, supervisors, inspectors. Use plain language. Include photographs if possible. Specify exactly what to do and what to look for. This is not a request for analysis; it's an instruction for immediate action.
**Effective Immediately; Until Rescinded**
**Affected Part(s):** {{part_number}}; {{part_description}}
**Affected Area(s):** {{production_areas}} (e.g., "Line 3; CNC Turning, Incoming Inspection, Final Inspection, Shipping")
2. {{action_2}} (e.g., "Segregate and tag any nonconforming parts found; do NOT scrap without Quality Engineering authorization")
Executive-level. Lead with the headline; is quality performance improving, stable, or deteriorating? Then provide the supporting data. Managers need to understand trend direction and business impact, not individual NCR details. Use charts and tables; minimize narrative.
**QUALITY MANAGEMENT REVIEW; {{review_period}}**
| Open; On Track | {{on_track}} |
| Open; Overdue | {{overdue}} |
1. {{action_request_1}} (e.g., "Approve capital expenditure for automated inspection system; ROI analysis attached")
- This is a critical document; it becomes part of the public FDA inspection record
- Unsupported disagreement or automatic admissions; acknowledge receipt and explain any evidence-supported position accurately
- "This was an isolated incident"; FDA explicitly looks for systemic issues
- "Employee has been terminated"; this is punitive, not corrective; FDA wants system fixes