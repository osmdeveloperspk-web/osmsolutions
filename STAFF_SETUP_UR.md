# Staff Management Setup (Urdu)

## 1) Database migration

Supabase Dashboard → SQL Editor میں یہ نئی migration مکمل execute کریں:

`supabase/migrations/202611070013_staff_management_permissions.sql`

یہ migration:
- `shop_members.permissions` شامل کرتی ہے اور موجودہ roles کے لیے default permissions set کرتی ہے۔
- `staff_invitations` بناتی ہے۔
- staff کے sign-up یا اگلے login پر اسی email کی pending invitation قبول کرتی ہے۔
- module-level permissions کو Supabase RLS اور حساس RPC functions پر enforce کرتی ہے۔
- موجودہ sales، inventory، purchases، returns یا exchange records کو delete یا reset نہیں کرتی۔

## 2) Application update

نئی frontend files اور تبدیلیاں اپنے project میں copy کریں، پھر معمول کے مطابق frontend deploy کریں۔

## 3) Staff شامل کرنے کا طریقہ

1. Owner account سے login کریں۔
2. Sidebar میں **Staff management** کھولیں۔
3. **Invite staff** پر کلک کرکے نام، email اور role منتخب کریں۔
4. Role کی default permissions ضرورت کے مطابق تبدیل کریں اور **Create invitation** کریں۔
5. یہ invitation email خودکار طور پر نہیں بھیجتی۔ Staff کو بتائیں کہ وہ اسی email سے OSM POS پر account بنائے یا login کرے۔
6. Login/sign-up پر pending invitation خودکار طور پر قبول ہو جائے گی اور staff کو صرف اجازت والے modules نظر آئیں گے۔

## 4) Role defaults

- **Manager:** تمام business modules؛ staff management صرف owner کے پاس۔
- **Cashier:** POS، inventory، customers اور returns/exchange۔
- **Technician:** repair، inventory اور customers۔
- **Staff:** customers تک محدود رسائی۔

Owner ہر staff member کی module permissions الگ سے تبدیل کر سکتا ہے۔

## 5) اہم نوٹ

- Staff invitation میں درج email اور login/sign-up email بالکل ایک جیسی ہونی چاہیے۔
- Existing staff user کو invitation بنانے کے بعد logout/login کرنا ہوگا تاکہ invitation accept ہو سکے۔
- پہلے اپنی staging/test shop پر migration اور staff roles test کریں۔ Production میں execute کرنے سے پہلے database backup رکھیں۔
- Email delivery خودکار نہیں ہے؛ اس کے لیے بعد میں Supabase Edge Function یا transactional email provider جوڑا جا سکتا ہے۔
