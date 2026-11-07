# OSM Mobile POS — Inventory module setup (Roman Urdu)

## Is update mein kya hai?
- Mobile Phones, Accessories aur Other predefined product types.
- Shop owner apni custom categories bana sakta hai.
- Other mein kisi bhi product ka apna naam likh kar add kiya ja sakta hai.
- Product name, brand, model, SKU/barcode, optional IMEI, new/used condition, quantity, purchase price, sale price aur low-stock alert.
- Product list, search, category filter, edit aur delete.
- Har record `shop_id` ke saath save hota hai; RLS shop membership enforce karti hai.

## Zaroori: Supabase migration ek martaba run karein
1. Supabase Dashboard → SQL Editor → New query.
2. Is project ki file `supabase/migrations/202611040002_inventory_module.sql` ka poora code paste karein.
3. Run karein. Ye additive migration hai: existing products ya shops ko drop/delete nahi karti.
4. Agar `Success. No rows returned` aaye to ye normal hai.

## Frontend update
1. Is ZIP ko purane project se alag folder mein extract karein.
2. `frontend/.env.local` mein apna Supabase URL aur Publishable Key verify karein.
3. CMD mein frontend folder khol kar:
   ```bash
   npm install
   npm run dev
   ```
4. `http://localhost:5173` open karein aur apne owner account se login karein.
5. Sidebar mein **Mobile inventory** par click karein.

## Notes
- Purani initial migration dobara run na karein; sirf nayi `202611040002_inventory_module.sql` migration run karein.
- Publishable key frontend mein theek hai. `service_role` / secret key kabhi frontend mein na daalein.
- Ye inventory ka pehla functional version hai. IMEI abhi optional single field hai; multi-IMEI-per-device, stock purchase ledger, barcode scanner integration aur sales ke saath automatic stock deduction aglay steps hain.
- Runtime integration ko aapke live Supabase project par verify karna abhi baqi hai.
