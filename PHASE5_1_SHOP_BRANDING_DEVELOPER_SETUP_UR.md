# Phase 5.1 — Shop Logo aur OSM Developers Shop Management

Ye update existing Supabase POS architecture ke saath integrate hota hai. Project mein Laravel Super Admin ka purana foundation bhi mojood hai, lekin woh alag MySQL tenant databases provision karta hai aur current Supabase POS records se linked nahi. Is live Supabase edition ke liye Laravel `/api/super/shops` endpoint use na karein; neeche diya gaya Supabase Platform Admin panel hi use karein. Existing Inventory, POS, Repair, Purchases, Customers, Returns, Mobile Exchange, Staff Management aur Phase 5 Activity Log data ko delete ya recreate nahi karta.

## Is update mein

- Shop Owner ko **Shop Branding** menu milega.
- Owner shop name update kar sakta hai aur PNG/JPG/WEBP logo upload/remove kar sakta hai.
- Logo sidebar mein shop title/branding ke taur par show hoga.
- OSM Developers ke authorized Platform Admin ko login ke baad **Shop Management** control panel milega.
- Platform Admin shop create karke owner ko Auth invitation bhej sakta hai.
- Agar owner email ka account pehle se mojood ho to nayi shop usi account ke saath link hoti hai.
- OSM Developers shop list dekh sakte hain aur shop ko Active/Suspended kar sakte hain.
- Suspended shop ka POS access database policies ke zariye block hota hai; activate karne par dobara access milta hai.

## 1. Backup aur migration

1. Supabase ka database backup/export lein.
2. Supabase Dashboard → SQL Editor kholein.
3. `supabase/migrations/202611070017_shop_branding_and_platform_admin.sql` ka poora SQL run karein.
4. Is se pehle Phase 5 tak ki migrations `013`, `014`, `015`, `016` applied honi chahiye. Purani migrations dobara run na karein.

## 2. OSM Developers Platform Admin account banana

1. Supabase Dashboard → Authentication → Users mein OSM Developers team ka dedicated login user create/invite karein.
2. SQL Editor mein ye query run karein; email ko usi admin email se replace karein:

```sql
insert into public.platform_admins (user_id)
select id from auth.users
where lower(email) = lower('YOUR_OSM_ADMIN_EMAIL')
on conflict (user_id) do update set is_active = true;
```

3. Har authorized OSM team member ke liye alag Auth user aur `platform_admins` entry banayein.
4. Shop owners ya normal staff ko `platform_admins` mein add na karein.

Platform admin role sirf trusted OSM Developers team ko dein. Ye control panel tamam customer shops ka administrative access deta hai.

## 3. Supabase Edge Function deploy

Project root folder mein Supabase CLI configured ho aur correct Supabase project linked ho. Phir run karein:

```bash
supabase functions deploy osm-admin-shops
```

Function ko service-role key server-side chahiye. Supabase Edge Functions ke built-in `SUPABASE_URL`, `SUPABASE_ANON_KEY` aur `SUPABASE_SERVICE_ROLE_KEY` environment variables use hote hain. **Service-role key frontend `.env.local`, Git, ya browser code mein kabhi paste na karein.**

Optional invite redirect URL set karein:

```bash
supabase secrets set APP_PUBLIC_URL=https://YOUR-POS-DOMAIN
```

`YOUR-POS-DOMAIN` ko actual deployed app URL se replace karein.

## 4. Email invitation settings

New owner email ke liye Supabase Auth → Email Templates mein Invite User template check karein. Production ke liye SMTP provider configure karein, warna invitation email deliver nahi ho sakti. Existing Auth account ko invite email ki zaroorat nahi hoti; shop us account se directly link ho jati hai.

## 5. Frontend deploy

1. Updated project ZIP extract karein.
2. Apni purani `frontend/.env.local` file preserve karein.
3. Frontend folder mein:

```bash
npm install
npm run build
npm run dev
```

4. Hosting par deploy karte waqt Vite environment variables (`VITE_SUPABASE_URL` aur `VITE_SUPABASE_PUBLISHABLE_KEY`) set rakhein aur fresh build/deploy karein.

## 6. Testing

### OSM Developers test

- Platform Admin email se login karein.
- Shop Management page open hona chahiye.
- Test shop name, owner name aur owner email ke saath shop create karein.
- New email ho to Auth invitation send honi chahiye (SMTP configured hona zaroori hai).
- Existing Auth email ho to shop us account ke saath link honi chahiye.
- Shop list mein shop aur owner details verify karein.
- Test ke liye shop suspend karein; owner/staff POS access blocked hona chahiye. Phir activate karke access restore verify karein.

### Shop Owner branding test

- Shop owner login kare.
- Sidebar se **Shop Branding** open kare.
- Shop logo upload karke Save Changes kare.
- Logo sidebar branding mein nazar aana chahiye.
- Logout/login ke baad bhi logo rehna chahiye.
- Staff account se Shop Branding option visible nahi hona chahiye.

## Important security notes

- Platform Admins ko manually trusted OSM team users tak limited rakhein.
- Public browser code mein service-role key na rakhein.
- Shop logos public branding images hain; CNIC ya private documents is bucket mein upload na karein.
- Phase 5 activity log aur tamam purane business records preserve rehte hain.
- Is phase mein plan/subscription billing, payment collection, automated expiry aur custom-domain provisioning shamil nahi; woh separate SaaS billing phase hoga.
