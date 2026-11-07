# OSM POS — Phase 4: Staff Action Permissions

## Default Cashier access
- POS par sale create kar sakta hai.
- Customer ledger mein customer payment receive kar sakta hai.
- Discount nahi de sakta.
- Sales return nahi kar sakta.
- Mobile exchange nahi kar sakta.
- Purchase/supplier balance settlement nahi kar sakta.
- CNIC documents view nahi kar sakta.
- Inventory management, Reports, Purchases, Returns aur Staff Management modules default taur par nahi milte.
- POS ke andar product catalogue read-only available hota hai; cashier Inventory screen se products edit nahi kar sakta.

Manager ko tamam listed actions default milte hain. Technician aur general Staff ko ye financial actions default taur par nahi milte. Owner har member ki permissions customize kar sakta hai.

## Database migration
1. Supabase database ka backup lein.
2. Ensure karein Phase 3 migrations `202611070013_staff_management_permissions.sql` aur `202611070014_staff_activation_security.sql` pehle apply ho chuki hain.
3. Supabase Dashboard → SQL Editor mein `supabase/migrations/202611070015_staff_action_permissions.sql` ka poora SQL run karein.
4. Migration ko ek hi baar run karein; script important policy/function definitions ko repeat-safe rakhne ke liye drop/replace karti hai.

## Frontend deployment
1. Is ZIP se updated frontend deploy karein.
2. Existing `frontend/.env.local` ko preserve karein. ZIP mein environment file jaan-boojh kar shamil nahi.
3. Local: `cd frontend` → `npm install` → `npm run build` → `npm run dev`.
4. Hosting: `VITE_SUPABASE_URL` aur `VITE_SUPABASE_PUBLISHABLE_KEY` environment variables set karke fresh redeploy karein.

## Existing Cashier members
Migration sirf bilkul untouched legacy Cashier preset ko safer preset mein badalti hai. Custom module permissions preserve hoti hain. Agar kisi purane Cashier ke paas ab bhi Inventory ya Returns module enabled ho, Owner → Staff Management → Edit member → Role mein Cashier dobara select karein → Save Changes.

## Test
- New Cashier invite karein; usay same invited email se account join/login karwayein.
- Staff permissions login par load hoti hain; owner ke changes ke baad staff ko logout/login karwayein.
- POS sale successful honi chahiye; discount zero/disabled ho.
- Customer payment receive available hona chahiye.
- Returns, exchange, settlement aur CNIC view unavailable/blocked hone chahiye.
- Manager/Owner se discount, returns, exchange, purchase settlement aur CNIC view regression test karein.

## Notes
- Database policies/RPC checks action-level enforcement karti hain; frontend controls sirf UI ko clear rakhte hain.
- CNIC image bucket private rehta hai; file read/download ke liye `cnic_view` required hai.
- Build is packaging environment mein verify nahi hua kyunke npm dependencies install nahi ho sakin. Deployment se pehle staging project aur database backup par test karein.
