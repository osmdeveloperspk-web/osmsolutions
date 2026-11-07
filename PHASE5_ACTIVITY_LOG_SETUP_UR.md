# OSM POS — Phase 5: Staff Activity & Audit Log

## Is phase mein kya add hua?
- Owner sidebar mein **Staff Activity Log** ka naya page.
- Sales, products, repairs, customer payments, purchases, supplier payments, expenses, returns, mobile exchanges aur staff/invitations ki create/update/delete activity record hoti hai.
- Har record ke saath waqt, actor email (agar authenticated session available ho), action, module/table aur record ID save hoti hai.
- Search aur Created / Updated / Deleted filters.
- Audit log sirf shop owner read kar sakta hai. Staff direct insert/update/delete nahi kar sakta.
- Sensitive customer/CNIC/payment details audit log mein copy nahi hotin.

## Migration apply karna
1. Pehle Supabase database ka backup lein.
2. Confirm karein ke Phase 4 migration `202611070015_staff_action_permissions.sql` apply ho chuki hai.
3. Supabase Dashboard → SQL Editor kholein.
4. `supabase/migrations/202611070016_staff_activity_audit_log.sql` ka poora SQL paste karke run karein.
5. SQL successful hone ke baad updated frontend deploy karein.

## Frontend update
- Updated ZIP se project files replace karein.
- Apni purani `frontend/.env.local` file preserve karein; ZIP mein real Supabase keys shamil nahi hotin.
- Local: `cd frontend`, `npm install`, `npm run build`, `npm run dev`.

## Test checklist
1. Owner login kare aur sidebar mein **Staff Activity Log** dekhe.
2. Staff account login kare; usay ye owner-only menu item nazar na aaye.
3. Staff se ek allowed action (misal: sale) karein; owner Activity Log refresh karke event dekhe.
4. Owner se staff permission update karein; membership change bhi log honi chahiye.
5. Kisi doosri shop ke logs access karne ki koshish RLS se block honi chahiye.

## Important
- Logs migration apply hone ke baad se record honge; purani activities import/backfill nahi hotin.
- Page latest 300 records load karta hai.
- Log mein full customer details, CNIC number, CNIC files, payment references ya notes store nahi kiye jate.
- Existing POS, Inventory, Returns aur Mobile Exchange workflows ko change nahi kiya gaya; sirf database audit triggers add kiye gaye hain.
