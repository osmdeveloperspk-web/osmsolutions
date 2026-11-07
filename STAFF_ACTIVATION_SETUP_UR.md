# Staff Activation & Role Verification — Setup (Urdu)

Is update mein staff invitation accept karne ka flow mazboot kiya gaya hai:

- Login ke baad app pehle shop membership aur invitation verify karti hai.
- Invitation sirf signed-in account ke apne authenticated email se claim hoti hai.
- Role aur permissions database se load hoti hain; header mein active role bhi nazar aata hai.
- Agar is email ke saath shop membership na mile to app ghalti se staff ko nayi shop banane par majboor nahi karti. Pehle exact invitation email se sign in karne ki hidayat deti hai.
- Internal invitation helper ko direct browser/client calls se band kiya gaya hai.

## Supabase migration

Agar migration `202611070013_staff_management_permissions.sql` pehle run ho chuki hai, to ab sirf ye nayi file run karein:

`supabase/migrations/202611070014_staff_activation_security.sql`

Supabase Dashboard → SQL Editor mein file ka poora SQL paste karke Run karein. Purani migration 013 ko dobara run na karein.

Agar migration 013 abhi tak run nahi hui, to pehle 013 aur uske baad 014 isi order mein run karein.

## Frontend update

Project ke `frontend/src/App.tsx` aur `frontend/src/styles.css` ko is ZIP ki updated files se replace karein. Baqi modules aur `frontend/.env.local` ko preserve karein. `frontend/.env.local` ZIP mein jaan boojh kar shamil nahi hoti.

Phir frontend folder mein:

```bash
npm install
npm run build
npm run dev
```

Live hosting par environment variables wahi rakhein aur fresh deployment karein.

## Test checklist

1. Owner account se login karein aur Staff Management mein pending invitation ka status check karein.
2. Staff member ko wahi exact email use karwaein jo invitation mein likhi hai.
3. Agar us email ka account nahi hai to pehle Sign Up kare; email confirmation required ho to confirmation ke baad Sign In kare.
4. Agar account pehle se hai to Sign Up ke bajaye Sign In kare.
5. Login ke baad app membership claim/verify karegi, shop kholegi aur assigned role header mein dikhayegi.
6. Test karein ke staff sirf assigned modules access kar sakta hai. Unauthorized module ke liye database RLS bhi access rokta hai.

**Note:** Invitation create karne se email automatically send nahi hoti. Owner ko staff member ko invitation wala exact email batana hota hai. Agar staff member pehle se logged in tha jab invitation create hui, to usay sign out karke dobara sign in karna hoga.
