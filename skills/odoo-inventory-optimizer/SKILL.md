  PICK ; Move goods from storage shelf to packing area
  PACK ; Package items and print shipping label
  OUT  ; Hand off to carrier / mark as shipped
- ✅ **Do:** Enable **Putaway Rules** on warehouses with multiple storage zones; verify suggested destinations and exceptions; rules do not eliminate all handling errors.
- ❌ **Don't:** Switch stock valuation method (FIFO ↔ AVCO) after recording transactions; it produces incorrect historical cost data.
- ❌ **Don't:** Use "Update Quantity" to fix stock errors; always use Inventory Adjustments to maintain a proper audit trail.
- Does not cover **landed costs** (import duties, freight allocation to product cost); that requires the `stock_landed_costs` module.