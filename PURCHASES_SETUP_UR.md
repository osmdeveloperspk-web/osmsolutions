# OSM Mobile POS — Purchase & Supplier Module (Phase 2)

## شامل فیچرز
- کسٹمر سے موبائل خریدتے وقت نام، والد کا نام، فون، CNIC نمبر، مکمل پتہ اور شہر محفوظ کرنا۔
- CNIC کے فرنٹ اور بیک کی تصویر لازمی اپ لوڈ کرنا۔ JPG، PNG، WEBP یا PDF؛ ہر فائل زیادہ سے زیادہ 5 MB۔
- ہول سیلر/سپلائر کا ریکارڈ، کمپنی، فون، پتہ اور شہر محفوظ کرنا۔
- موبائل، ایکسیسری یا دوسری چیز کی خریداری؛ موبائل کے لیے ہر فون الگ اور IMEI لازمی ہے۔
- خریداری کی رقم، ادا شدہ رقم اور بقایا محفوظ کرنا۔
- کامیاب خریداری پر اسی وقت `products` انوینٹری میں اسٹاک شامل کرنا۔
- خریداری کی ہسٹری، سپلائر لیجر اور سپلائر کو بعد میں ادائیگی ریکارڈ کرنا۔
- English/Urdu UI۔

## Supabase migration لازمی ہے
1. Supabase Dashboard کھولیں۔
2. اپنے project میں **SQL Editor** کھولیں۔
3. نئی ZIP کے `supabase/migrations/202611060008_purchase_supplier_management.sql` کی مکمل SQL کاپی کریں۔
4. SQL Editor میں پیسٹ کرکے **Run** کریں۔
5. Storage میں `purchase-cnic` نام کا private bucket خود migration بنائے گی۔ اسے Public نہ کریں۔

> یہ migration موجودہ ٹیبلز/ریکارڈز حذف نہیں کرتی؛ نئے ٹیبلز، policies، private bucket اور RPC functions شامل کرتی ہے۔ اسے ایک ہی Supabase project پر صرف ایک بار چلائیں۔

## Frontend چلائیں
```bash
cd frontend
npm install
npm run dev
```

## Customer Purchase استعمال کرنے کا طریقہ
1. Sidebar سے **Purchases & suppliers** کھولیں۔
2. **Customer purchase** منتخب کریں۔
3. Seller کی تمام تفصیلات پُر کریں اور CNIC کے دونوں رخ اپ لوڈ کریں۔
4. فون کا نام، برانڈ، ماڈل، حالت، IMEI، خرید قیمت اور متوقع فروخت قیمت درج کریں۔
5. آج ادا کی گئی رقم درج کرکے خریداری محفوظ کریں۔
6. کامیاب خریداری کے بعد فون Inventory میں شامل اور Purchase History میں نظر آئے گا۔

## Supplier Purchase
1. **Supplier purchase** منتخب کریں۔
2. پہلے supplier منتخب کریں یا **Add supplier** سے نیا supplier بنائیں۔
3. آئٹم، IMEI/quantity، قیمت اور ادائیگی درج کرکے محفوظ کریں۔
4. **Suppliers & ledger** میں خریداری، ادا شدہ رقم اور بقایا دیکھیں اور بقایا ادائیگی درج کریں۔

## CNIC اور پرائیویسی
- CNIC فائلیں `purchase-cnic` کے **private** bucket میں محفوظ ہوتی ہیں۔
- فائل کا path ڈیٹا بیس میں محفوظ ہوتا ہے، public URL نہیں۔ دیکھنے کے لیے صرف 60 سیکنڈ کی signed URL بنائی جاتی ہے۔
- Storage policies ہر فائل کے پہلے path segment کو shop UUID سے ملاتی ہیں اور صرف اسی shop کے authenticated members کو اجازت دیتی ہیں۔
- CNIC تصاویر صرف قانونی/ضروری مقصد اور مجاز عملے کے ساتھ استعمال کریں۔ غیر ضروری طور پر تصاویر ڈاؤن لوڈ یا شیئر نہ کریں۔

## اہم نوٹ
- فون کا IMEI لازمی ہے اور ایک ہی دکان کے موجودہ inventory یا purchase records میں duplicate IMEI قبول نہیں ہوگا۔
- ہر فون الگ purchase entry ہوگا؛ ایک phone entry میں quantity ہمیشہ 1 ہے۔ Accessories/other items کے لیے quantity ایک سے زیادہ ہو سکتی ہے۔
- اس ورژن میں purchase receipt PDF/print اور customer-seller کے بقایا کی بعد کی settlement ابھی شامل نہیں؛ یہ اگلے مرحلے کے کام ہیں۔
