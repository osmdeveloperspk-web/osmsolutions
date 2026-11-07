# Customers Management — Setup (Urdu)

## 1) Supabase migration

Supabase Dashboard → SQL Editor → New query کھولیں۔ پھر یہ فائل مکمل copy/paste کرکے Run کریں:

`supabase/migrations/202611050006_customers_credit_ledger.sql`

یہ migration `customers` اور `customer_payments` tables بناتی ہے، موجودہ `sales` table میں customer/paid/balance fields شامل کرتی ہے، اور shop-wise RLS policies لگاتی ہے۔ پہلے سے موجود sales کو محفوظ رکھنے کے لیے انہیں مکمل paid شمار کیا جاتا ہے؛ پرانی sales پر خودکار ادھار نہیں بنایا جاتا۔

## 2) Frontend چلائیں

`frontend` folder میں:

```bash
npm install
npm run dev
```

Login کے بعد sidebar میں Customers & staff پر جائیں۔

## 3) ابتدائی ٹیسٹ

1. ایک test customer بنائیں، مثلاً Test Customer۔
2. Opening balance میں 1,000 درج کریں۔
3. Customer کے Ledger بٹن سے ledger کھولیں۔
4. Receive payment میں 300 درج کرکے Cash منتخب کریں۔
5. Save کے بعد بقایا 700 ہونا چاہیے اور payment history میں 300 کی الگ entry نظر آنی چاہیے۔

نوٹ: اس مرحلے میں opening balance اور customer payment ledger کام کرتے ہیں۔ POS checkout کو customer_id کے ساتھ link کرنا اور نئی sales کی partial/credit payment allocation اگلے integration step میں شامل ہوگی۔
