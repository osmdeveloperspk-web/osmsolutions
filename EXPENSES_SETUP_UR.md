# OSM Mobile POS — Expenses Management Setup

## 1. Supabase database setup
1. Open Supabase Dashboard → SQL Editor → New Query.
2. Open `supabase/migrations/202611050005_expenses_management.sql` from this project.
3. Copy the complete SQL, paste it into SQL Editor and click **Run**.
4. Expected result: `Success. No rows returned`.

This migration creates `public.shop_expenses`, an index and a tenant-membership RLS policy. It does not delete or modify existing sales, stock, repair, customer or shop records.

## 2. Run the updated frontend
1. Extract this ZIP.
2. Open the extracted `osm_pos_work/frontend` folder.
3. Keep your existing `frontend/.env.local` Supabase settings. The ZIP includes the current project settings, but if your local values differ, preserve your working `.env.local`.
4. Open Command Prompt in `frontend` and run:

```bash
npm install
npm run dev
```

5. Sign in and select **Expenses Management** from the left menu.

## 3. Test
Add one test expense (for example, category `Other`, description `Test expense`, amount `100`, payment method `Cash`). Confirm it appears in the list and the Today / This month totals update. Then edit it and delete it if it was only a test.

## Features
- Add, edit and delete expenses
- Date, category, description, amount, payment method, paid-to and reference fields
- Search and category filter
- Today and current-month totals
- English / Urdu labels
- Shop-specific records protected by Supabase Row Level Security
