# Phase 3 — Sales Returns & Exchange

1. Apply `supabase/migrations/202611060010_sales_returns_exchange.sql` in Supabase SQL Editor after migrations 008 and 009.
2. Extract the complete project ZIP, then in `frontend` run `npm install` and `npm run dev`.
3. Open **Sales Returns & Exchange** from the sidebar.
4. Search an exact original invoice number, select an item, quantity, refund amount/method and reason, then process the return.
5. Confirm stock increased by the returned quantity and the return appears in Recent Returns.

Rules:
- Only items on a real sale invoice can be returned.
- Previously returned quantities are subtracted from the eligible quantity.
- Refund cannot exceed the original unit price multiplied by return quantity.
- Return record, return item and inventory restock occur atomically in the database function.
- Exchange workflow is currently supported as a return reason; completing the replacement sale remains a separate POS checkout. Do not treat this screen as a single atomic exchange transaction.


## Phase 3.1 — Mobile Exchange (Trade-in)
1. Apply `supabase/migrations/202611060011_mobile_exchange_management.sql` after migration 010. If it was already applied, also apply the corrective migration `supabase/migrations/202611070012_mobile_exchange_settlement_fix.sql`.
2. Restart frontend (`npm run dev`) after replacing the project files.
3. In Sales Returns & Exchange, search the original customer invoice.
4. Use the Exchange panel: choose the old sold item, enter agreed trade-in value, select the replacement mobile from available stock, set its quantity and agreed selling price, enter staff/dealer name and payment method.
5. Review the live difference calculation before pressing Complete mobile exchange.
6. The database function performs the trade-in return/restock and replacement sale/stock deduction in one transaction. It creates a replacement invoice and exchange record.
7. Test with test inventory and a test sale first. Do not use a live customer transaction until the SQL migration and stock/invoice results are verified.

If replacement total is higher than trade-in value, the customer pays the difference. The replacement invoice records only this cash difference as received, while its balance due remains zero because the trade-in credit settles the rest. If trade-in value is higher, the screen shows the amount the shop owes the customer; refund that amount to the customer and retain the exchange record, which stores the signed difference. The database function returns `cash_difference_received` and `shop_refund_due` for verification.
