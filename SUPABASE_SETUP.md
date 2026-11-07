# OSM Mobile POS — Supabase setup

1. The delivered project includes `frontend/.env.local` populated with the project URL and Publishable Key supplied by the owner. If you replace these values, keep the variable names exactly as shown in `frontend/.env.example`. This file is local configuration and must not be committed to a public Git repository.
2. In Supabase Dashboard open **SQL Editor**, create a new query, paste the contents of `supabase/migrations/202611040001_initial_saas_schema.sql`, and run it.
3. In the project folder run `cd frontend`, then `npm install`, then `npm run dev`.
4. Click **Test Supabase connection** in the dashboard. A successful auth session lookup verifies that the frontend can reach the Supabase Auth endpoint; it does not yet mean the business tables are fully wired into screens.

## Security
- The frontend uses only the Supabase Publishable Key. Never expose `service_role` or secret keys.
- RLS is enabled on the initial tables. Review and extend policies before production.
- This is an initial shared-database tenant schema (`shop_id` + membership RLS), not one physical database per shop.
- Customer CNIC scans/photos must use a private Storage bucket with strict policies and must not be publicly accessible. That bucket is not created by this initial migration.

## Current scope
Sidebar navigation now opens module placeholder pages, and the dashboard includes a Supabase connectivity test. Product CRUD, IMEI management, sales checkout, repair workflow, customer records, staff invitation, offline sync and reporting still require implementation and testing.

## Inventory module update
After the initial schema has already been applied, run only `supabase/migrations/202611040002_inventory_module.sql` once in Supabase SQL Editor. See `INVENTORY_SETUP_UR.md` for Roman Urdu instructions. Do not rerun the initial migration.
