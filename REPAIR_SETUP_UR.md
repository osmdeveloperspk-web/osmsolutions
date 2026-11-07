# OSM Mobile POS — Repair Management setup

## Update
This update adds the Repair Management page to the existing POS & Inventory project. It keeps the existing Supabase project and shop data.

## Supabase migration (required once)
1. Open Supabase Dashboard → SQL Editor → New query.
2. Open `supabase/migrations/202611050004_repair_management.sql` from this ZIP.
3. Paste the full SQL and click Run.
4. Confirm the query reports Success.

Do not rerun the older migrations. This migration upgrades the existing `repair_tickets` table and does not drop tables or delete records.

## Run locally
Open a terminal in `frontend`:

```bash
npm install
npm run dev
```

Open `http://localhost:5173`, sign in with the existing owner account, then choose **Repair Management** from the left menu.

## Included
- Create and edit repair tickets with automatically generated ticket numbers.
- Customer name and phone, device brand/model, IMEI, fault and repair notes.
- Parts cost, labour cost, customer charge, advance and balance due.
- Repair status tracking: Received, In Progress, Waiting for Parts, Completed, Delivered, Cancelled.
- Manual dealer/technician name.
- PKT date/time display, search, status filter, summary and printable repair receipt.
- Supabase tenant isolation through the existing shop membership RLS policy.

The migration is designed for the previous POS update's existing `repair_tickets` table. Run it before opening the Repair Management page.
