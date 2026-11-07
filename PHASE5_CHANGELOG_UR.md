# Phase 5 Changelog

- Added owner-only Staff Activity Log navigation and page.
- Added search by actor email, module/table name and record ID.
- Added Created / Updated / Deleted filters.
- Added shop-scoped immutable audit table protected by RLS.
- Added database triggers for key business tables and staff membership/invitation changes.
- Audit rows store metadata only, not customer/CNIC/payment payloads.
- Existing business workflows were left unchanged.
