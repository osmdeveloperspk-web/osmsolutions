# OSM Mobile POS — POS & Sales Setup (Urdu)

Is update mein POS billing screen add ki gayi hai. Existing login aur inventory files/data ko delete nahi kiya gaya.

## 1) Supabase database migration chalayein

1. Supabase Dashboard kholen → SQL Editor → New query.
2. Is ZIP ke `supabase/migrations/202611040003_pos_sales_module.sql` ki **poori** file kholen aur copy karein.
3. SQL Editor mein paste karein aur Run karein.
4. Neeche `Success. No rows returned` ya successful completion ka message aana chahiye.

Ye migration `sales` aur `sale_items` tables banati hai aur `complete_pos_sale` function add karti hai. Checkout ke waqt stock transaction ke andar safely minus hota hai. Existing `products` aur unka data delete nahi hota.

## 2) Frontend update chalayein

1. Is ZIP ko extract karein.
2. `frontend` folder mein CMD/Terminal kholen.
3. Commands chalayein:

```bash
npm install
npm run dev
```

4. Browser mein `http://localhost:5173` kholen aur login karein.
5. Left menu → **POS & sales**.

## 3) POS ka istemal

- Search se product add karein.
- Quantity aur unit sale price adjust kar sakte hain.
- Customer name/mobile optional hain.
- Dealer/shopkeeper name lazmi hai; yahan us employee ka naam likhein jis ne sale ki.
- Discount aur payment method select karein.
- Complete sale par invoice number generate hota hai, date/time Pakistan time (Asia/Karachi) mein dikhte hain aur stock automatically reduce hota hai.
- Successful sale ke baad **Print invoice** se invoice print karein.

## Zaroori notes

- Supabase URL aur publishable key `frontend/.env.local` mein project ki local configuration ke mutabiq honi chahiye. Publishable key browser mein use karne ke liye hoti hai; **service-role/secret key kabhi frontend mein na rakhein**.
- Is build mein English/Urdu interface, manual dealer name, customer fields, payment method, invoice date/time, totals/discount aur stock deduction shamil hain.
- Offline POS sync, tax configuration, barcode scanner hardware integration, reprint from historical invoice list, returns/refunds aur full dealer-wise analytics abhi next development items hain.
- Is environment mein npm dependency installation/build complete nahi ho saka; is liye is ZIP ko production-ready ya fully tested na samjhein. Pehle local test sale karein aur stock/invoice verify karein.


## Customer linking and partial / credit checkout (V2)

Apply `supabase/migrations/202611050007_pos_customer_credit_checkout.sql` in Supabase SQL Editor after the customer credit ledger migration. This updates the atomic checkout RPC to accept a customer and amount received, save the remaining balance on the invoice, and preserve stock updates in the same transaction. Partial or credit sales require selecting a registered customer. Full payment remains the default.
