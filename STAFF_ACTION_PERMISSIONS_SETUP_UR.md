# OSM POS — Staff Action Permissions (Phase 4)

## Is update mein kya hai
- Staff invite aur member record mein `action_permissions` save hoti hain.
- Cashier ka default access: POS sale + customer payment receive.
- Cashier ko default taur par Inventory management, returns, mobile exchange, discounts, purchase settlements, CNIC document viewing aur reports ki access nahi milti.
- Manager ko ye actions default taur par milte hain; technician aur general staff ko nahi.
- Important actions par Supabase RLS / RPC checks lagaye gaye hain. Sirf frontend buttons hide karna security ke liye kaafi nahi hota.
- POS cashier product list dekh sakta hai, lekin Inventory screen se product edit nahi kar sakta. Checkout stock ko atomic RPC ke zariye update karta hai.
- CNIC image files private rehti hain; unhein read/download karne ke liye `cnic_view` action zaroori hai.

## Supabase migration
1. Database ka backup lein.
2. Supabase Dashboard → SQL Editor kholein.
3. File `supabase/migrations/202611070015_staff_action_permissions.sql` ka poora SQL run karein.
4. Is se pehle Phase 3 staff migrations `013` aur `014` apply honi chahiye.
5. Migration ko ek hi martaba run karein.

## Frontend update
- Updated project ZIP se frontend files deploy karein.
- Apni existing `frontend/.env.local` file preserve karein; is file ko ZIP mein security ki wajah se include nahi kiya gaya.
- Local development: `cd frontend`, `npm install`, `npm run build`, `npm run dev`.
- Hosted deployment mein wahi `VITE_SUPABASE_URL` aur `VITE_SUPABASE_PUBLISHABLE_KEY` hosting provider ke environment variables mein rakhein aur fresh redeploy karein.

## Existing Cashier accounts
Migration sirf un purane Cashier presets ko safe defaults mein badalti hai jo bilkul original default permissions par thay. Customized permissions ko overwrite nahi kiya jata. Agar kisi purane Cashier ko Inventory ya Returns ki purani access ab bhi mil rahi ho, Owner → Staff Management → member edit karein, role dropdown mein Cashier dobara select karein, phir Save Changes karein. Is se new Cashier preset apply ho jayega.

## Test checklist
1. Owner se naya Cashier invite karein.
2. Invite accept hone ke baad staff ko logout/login karwayein (permissions login par reload hoti hain).
3. Cashier ko POS aur Customers modules nazar aane chahiye; Inventory, Returns, Reports, Purchases aur Staff Management nahi.
4. POS par discount field disabled/zero honi chahiye.
5. Customer ledger mein Receive Payment available hona chahiye.
6. Direct database/RPC request se discount, return, exchange, purchase settlement ya CNIC file read karne ki koshish bhi denied honi chahiye.
7. Manager/Owner ke existing workflows aur Mobile Exchange ko regression test karein.

## Important
- Owner kisi staff member ki module aur action permissions ko manually customize kar sakta hai.
- Staff permission change ke baad staff ko logout/login ya page reload karwana zaroori hai.
- Build aur live Supabase migration is development environment mein execute nahi ki gayi; deploy se pehle staging/backup par test karein.
