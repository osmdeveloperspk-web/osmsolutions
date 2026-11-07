# Phase 5.1 Changelog

## Added
- Shop Branding owner screen for shop name and logo upload/removal.
- Shop logo shown in the POS sidebar brand/title.
- `logo_url`, `owner_phone`, and `status` fields on existing shops.
- Public shop-logo Storage bucket with owner-scoped upload/update/delete policies and 2MB image limit.
- OSM Developers Platform Admin login detection and Shop Management dashboard.
- Secure Supabase Edge Function to list shops, create a shop, invite/link its owner, and activate/suspend shops.
- `platform_admins` allowlist; only manually authorized OSM team accounts can use the developer console.
- Shop creation through the browser is disabled; new customer shops are provisioned by OSM Developers.
- Suspended shop access is blocked through shared database permission helper functions.

## Preserved
- Phase 5 Activity Log and all previous modules/data.
- Staff invitations, role permissions, action permissions, POS, Inventory, Purchases, Customers, Returns, Repair and Mobile Exchange workflows.

## Required setup
- Apply migration `202611070017_shop_branding_and_platform_admin.sql`.
- Add OSM team Auth user IDs to `public.platform_admins`.
- Deploy Edge Function `osm-admin-shops`.
- Configure Supabase Auth invite email/SMTP and `APP_PUBLIC_URL`.
- Deploy updated frontend while preserving `frontend/.env.local`.

## Verification status
- TypeScript/TSX syntax parsing passed for changed frontend files and Edge Function.
- Full frontend build and live Supabase migration/function execution were not run in this environment because npm dependencies and the user's Supabase project credentials are not available here.
