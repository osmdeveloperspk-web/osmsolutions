# Phase 4 Changes

- Staff Management now saves granular `action_permissions` on invitations and memberships.
- Cashier preset updated to POS + customer payments only, with product catalogue read-only access inside POS.
- Discount, sales return, mobile exchange, customer payment, purchase settlement and CNIC document access have separate action keys.
- Supabase RLS and RPC checks enforce critical actions; UI hides or disables the corresponding controls.
- POS checkout now uses a narrowly permission-checked atomic function so a cashier can decrement stock without general inventory editing.
- Existing Mobile Exchange business workflow was not rewritten; only its action authorization was tightened.
