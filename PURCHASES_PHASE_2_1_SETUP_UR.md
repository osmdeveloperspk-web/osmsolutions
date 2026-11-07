# OSM Mobile POS — Purchase Phase 2.1 (اردو)

اس اپڈیٹ میں Purchase History کے لیے تین فیچرز شامل ہیں:

1. **Purchase Invoice:** ہر خریداری کے سامنے Print آئیکن سے رسید کھولیں، پھر Print یا Save as PDF کریں۔
2. **CNIC Verification:** کسٹمر کی خریداری میں دونوں CNIC تصاویر دیکھنے کے بعد Verify CNIC دبائیں۔ نوٹ اختیاری ہے؛ تصدیق کی تاریخ اور تصدیق کرنے والے صارف کا ریکارڈ محفوظ ہوتا ہے۔ دوبارہ دبانے سے verification واپس بھی کی جا سکتی ہے۔
3. **Customer Seller Settlement:** کسٹمر سے خریدے گئے فون کی بقایا رقم ادا کرنے کے لیے Settle دبائیں۔ ادائیگی کی رقم، طریقہ اور حوالہ نمبر درج کریں۔ سسٹم بقایا رقم اپڈیٹ کرے گا اور ادائیگی کی الگ ہسٹری محفوظ کرے گا۔

## لازمی Supabase Migration

Supabase Dashboard → SQL Editor میں یہ فائل مکمل Run کریں:

`supabase/migrations/202611060009_purchase_invoice_settlement_verification.sql`

یہ migration پہلے والی purchases tables کو حذف یا recreate نہیں کرتی؛ صرف verification columns، payment history table، RLS policy اور دو محفوظ RPC functions شامل کرتی ہے۔

## ٹیسٹ کرنے کا طریقہ

- Purchase History میں کسی بھی ریکارڈ کا Print آئیکن دبائیں؛ پرنٹ ونڈو میں رسید اور Save as PDF دستیاب ہونا چاہیے۔
- Customer purchase کی CNIC front/back دونوں تصاویر کھولیں، پھر Verify CNIC کریں؛ status Verified دکھنا چاہیے۔
- ایسا customer purchase منتخب کریں جس کا Balance Due صفر سے زیادہ ہو، جزوی settlement درج کریں؛ Paid بڑھے اور Balance Due کم ہو۔ مکمل settlement پر Balance Due صفر ہونا چاہیے۔
- ایک ہی بقایا رقم دوبارہ ادا کرنے کی کوشش کریں؛ RPC اسے روک دے گا۔

CNIC تصاویر بدستور private `purchase-cnic` bucket میں ہیں اور signed URL صرف محدود مدت کے لیے بنتا ہے۔ اس مرحلے میں CNIC نمبر یا تصاویر invoice میں شامل نہیں کی جاتیں تاکہ شناختی معلومات غیر ضروری طور پر پرنٹ نہ ہوں۔
