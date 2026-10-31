> - **Manufacture This Product**; standard production BoM, creates a Manufacturing Order
> - **Kit**; sold as a bundle; components are delivered separately (no MO created)
> - **Subcontracting**; components are sent to a subcontractor who returns the finished product
  "Replenish"  ; Stock is below minimum; needs a PO or MO
  "Reschedule" ; An order's scheduled date conflicts with demand
  "Cancel"     ; Demand no longer exists; the order can be cancelled
- ✅ **Do:** Use **BoM with variants** (via product attributes) for products that come in multiple configurations (color, size, voltage); avoids duplicate BoMs.
- ✅ **Do:** Use **Scrap Orders** when discarding defective components during production; never adjust stock manually.
- ❌ **Don't:** Manually create purchase orders for MRP-managed items; override MRP suggestions only when justified.
- ❌ **Don't:** Confuse **Kit** BoM with **Manufacture This Product**; a Kit never creates a Manufacturing Order.
- MRP calculations assume demand comes from **Sale Orders** and **Reordering Rules**; forecasts from external systems require custom integration.