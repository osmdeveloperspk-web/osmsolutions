# OSM Mobile POS — Owner Registration & Login (Supabase)

## Aapke liye setup
1. Is ZIP ko purane folder se alag extract karein.
2. `frontend/.env.local` mein aapka Supabase URL aur Publishable Key configured hain.
3. Supabase Dashboard → Authentication → URL Configuration mein Site URL ko `http://localhost:5173` set karein (development ke liye).
4. Email confirmation ON hai to signup ke baad inbox se email confirm karein. Phir login karein. Confirmation email ka redirect URL bhi `http://localhost:5173` rakhein.
5. Project ke `frontend` folder mein CMD khol kar chalayein:
   ```bash
   npm install
   npm run dev
   ```
6. Browser mein `http://localhost:5173` kholein. Create shop account par owner name, shop name, email aur password dein.
7. Agar confirmation email aati hai to confirm karke login karein. Shop dashboard par aapki shop show hogi.

## Important
- Supabase SQL migration pehle hi run kar chuke hain to dobara run karne ki zaroorat nahi.
- Publishable key browser app mein use karne ke liye hai. `service_role` ya secret key frontend mein kabhi na daalein.
- Shop record aur owner membership create karne ke liye logged-in user ki zaroorat hai. Is liye email confirmation enabled ho to pehle email confirm karke login karna hoga.
- Agar Auth → URL Configuration mein localhost allowed na ho to confirmation link wapas app par nahi aa sakta.
- Ye release owner signup/login aur shop creation foundation hai; POS, stock, IMEI, repairs aur accounts ke actual workflows abhi aglay phases mein develop honge.
