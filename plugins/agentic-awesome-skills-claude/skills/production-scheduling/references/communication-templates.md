# Communication Templates: Production Scheduling
> **Reference Type:** Tier 3; Load on demand when composing or reviewing production scheduling communications.
| `{{line_id}}` | Production line identifier | `Line 3; CNC Machining Cell` |
| `{{plant}}` | Plant name/location | `Cincinnati Plant; Building 2` |
| `{{constraint_wc}}` | Constraint work centre | `CNC Horizontal Boring; WC 420` |
**Subject:** Production Schedule; {{plant}}; {{shift}}; {{date}}
| 1 | {{work_order}} | {{product}} | {{qty}} | 06:00 | 08:30 | {{line_id}} | {{operator_name}} | 🔴 | Rush; customer line-down |
- WO-04826: Bracket raw material pending; confirm with stores by 10:00 ⚠️
**Constraint Status:** {{constraint_wc}}; current OEE {{oee_value}}. Buffer status: GREEN.
**Subject:** ⚠️ SCHEDULE CHANGE; {{line_id}}; Effective {{effective_time}}
**Why:** {{detailed_reason}}; e.g., "WO-04826 material (bracket raw stock) arrived early. WO-04826 due date is 1 day earlier than WO-04824. The illustrated transition increases from15 to20minutes, adding5minutes. Recalculate the entire sequence, setup state, durations and downstream effects before claiming a benefit."
**Tone guidance:** Authoritative but explanatory. The "why" is important because frequent unexplained changes erode shop floor trust in the schedule. Always include who approved the change (accountability). End with a stability commitment; "no further changes"; to prevent the shop floor from anticipating constant flux.
**Delivery:** Immediate; phone/radio for constraint events, email for non-constraint
**Subject:** 🔴 DISRUPTION ALERT; {{disruption_type}} at {{line_id}}; {{date}} {{time}}
- **Throughput loss:** {{throughput_loss}} (e.g., "$4,800; 6 hours × $800/hr constraint throughput")
2. {{verified_parts_order_status}}; ETA {{pump_eta}}.
**Tone guidance:** Lead with impact, not description. The production manager needs to know "how bad is this?" before "what exactly happened." Quantify everything in hours and dollars. Present decisions as explicit Y/N choices; do not leave it ambiguous. Set a next-update cadence so management isn't chasing you for information.
**Subject:** Overtime Request; {{line_id}}; {{date_range}}
| Personnel names | {{personnel_names}} (voluntary; confirmed availability) |
**Tone guidance:** Treat overtime requests as business cases, not pleas. Quantify both the cost and the benefit. Include union compliance confirmation proactively; the production manager should not have to ask. Provide the approval deadline because overtime notification requirements are contractual, not flexible.
**Subject:** Delivery Impact; {{customer}}; Order {{customer_po}}; Revised ETA {{revised_date}}
**Subject:** Delivery Update; Order {{customer_po}}
**Tone guidance; internal:** Factual, quantified, includes penalty exposure. The account manager needs the full picture to make the right call on messaging.
**Tone guidance; external:** Proactive (before the customer discovers the delay), accountable (acknowledge the impact), action-oriented (show what you're doing), no blame (do not name internal equipment or personnel). Never use "we apologise for any inconvenience"; that phrase signals insincerity. Instead, acknowledge the specific impact on their operations.
**Subject:** Maintenance Window Request; {{line_id}}; {{requested_date_range}}
- Lost production during the PM window: {{lost_production}} (e.g., "0; Saturday is non-scheduled overtime; if OT was planned, 8 hours of production displaced")
**Subject:** 🔴 QUALITY HOLD; {{product}}; Batch {{batch_id}}; {{qty_affected}} units
Example: "Dimensional defect on stamped chassis frames; hole pattern shifted 2mm from specification due to suspected die wear. Discovered at weld inspection station."
- If not reworkable: replacement production order required; estimated lead time {{replacement_lead_time}}
3. Production schedule revised; constraint fed from alternate work orders
**Subject:** Capacity Overload Alert; {{constraint_wc}}; Week {{week_number}}
| Subcontract {{subcontract_ops}} | {{subcontract_hours}} hrs | ${{subcontract_cost}} | Medium; quality and lead time | Last resort |
**Recommended plan:** Combine overtime ({{ot_hours}} hrs) + changeover reduction ({{co_hours}} hrs) to close the gap. Total gap closed: {{total_recovered}} hrs. Remaining gap: {{remaining_gap}} hrs; address by deferring {{defer_count}} Tier-3 orders with customer agreement.
**Subject:** NPI Trial Run Request; {{npi_product}}; {{requested_dates}}
**Product:** {{npi_product}} (e.g., "EV Battery Enclosure; Part #BE-4400")
- {{buffer_hours}} hrs/week reserved as "trial buffer"; converts to regular production if trial is cancelled or completes early
**Subject:** Priority Alignment Request; Week {{week_number}} / Month {{month}}
| 1 | Customer A rush order vs. Customer B committed delivery; both need CNC constraint, 16-hour gap | Sales + Production | Need commercial decision: which customer takes priority? |
| 2 | NPI trial run vs. production schedule; trial requires 14 hrs of constraint time in a week loaded at 94% | Engineering + Production | Recommend scheduling trial on Saturday to avoid displacement |
| 3 | Maintenance PM window vs. peak production week; PM deferred twice already | Maintenance + Production | Recommend executing PM this week; deferral risk exceeds production value of the PM window |
**Tone guidance:** Neutral facilitator, not advocate. The scheduler's role in priority alignment is to surface conflicts, quantify tradeoffs, and execute decisions; not to make commercial or strategic calls. Make it clear that you need a decision, not a discussion. Provide the data that enables the decision.