This skill guides you through the complete Odoo Purchase workflow; from sending a Request for Quotation (RFQ) to receiving goods and matching the vendor bill. It also covers purchase agreements, vendor price lists on products, automated reordering, and 3-way matching controls.
- ❌ **Don't:** Confirm a PO before prices are agreed; use Draft/RFQ status to negotiate first.
- ❌ **Don't:** Delete a PO that has received quantities; archive it instead to preserve the stock and accounting trail.
- Does not cover **subcontracting purchase flows**; those require the Manufacturing module and subcontracting BoM type.
- **EDI-based order exchange** (automated PO import/export) requires custom integration; use `@odoo-edi-connector` for that.