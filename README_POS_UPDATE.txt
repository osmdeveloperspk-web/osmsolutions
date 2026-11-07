OSM MOBILE POS - POS & SALES UPDATE

1. First run the SQL migration file in Supabase SQL Editor:
   supabase/migrations/202611040003_pos_sales_module.sql
2. Extract the ZIP and open frontend folder in CMD.
3. Run: npm install
4. Run: npm run dev
5. Open http://localhost:5173 and sign in.
6. Open POS & sales from the left menu.

Read POS_SETUP_UR.md for Urdu step-by-step instructions.

IMPORTANT: Existing products/inventory are preserved. The migration is additive.
The POS checkout requires the migration to be run before completing sales.
