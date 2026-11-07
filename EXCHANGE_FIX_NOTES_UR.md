# Mobile Exchange Fix — Urdu Notes

Is update mein:
- Exchange quantity ko normal Return quantity se alag state mein rakha gaya hai.
- Invoice search ke baad pehla aisa sold item select hota hai jis ki unreturned quantity mojood ho.
- Fully returned item par wazeh warning aati hai; aisi item dobara exchange nahi ho sakti.
- Old aur replacement quantities integer aur available stock ke mutabiq validate hoti hain.
- Settlement migration replacement invoice par sirf customer se milne wala net cash difference `amount_paid` mein record karti hai aur trade-in credit ke baad invoice ka `balance_due` zero rakhti hai.
- Agar trade-in value replacement price se zyada ho to result mein `shop_refund_due` return hota hai; shop ko woh amount customer ko manually refund karna hoga.

## Supabase migration

Agar migration 011 pehle apply nahi ki, pehle yeh run karein:
`supabase/migrations/202611060011_mobile_exchange_management.sql`

Us ke baad yeh corrective migration run karein:
`supabase/migrations/202611070012_mobile_exchange_settlement_fix.sql`

Agar 011 pehle se apply hai to sirf 012 run karein. Migration 012 ko dobara run karna safe hai (`create or replace function`).

## Test

1. Aisi original invoice search karein jis ki selected mobile quantity pehle return na hui ho.
2. Old phone quantity aur agreed trade-in value enter karein.
3. Replacement mobile, quantity, unit price aur staff/dealer name select karein.
4. Difference verify karein aur exchange complete karein.
5. Confirm karein: old stock barha, replacement stock ghata, return item record bana, replacement invoice bani aur `mobile_exchanges` mein exchange record bana.
6. Agar old mobile pehle hi fully return ho chuka ho to usi quantity ko exchange karna allowed nahi hoga. Test ke liye fresh invoice use karein.

**Note:** Is environment mein npm dependencies install nahi ho sakin, is liye frontend production build run nahi ho saka. Project ke `frontend` folder mein `npm install` aur phir `npm run build` chala kar local build verify karein.
