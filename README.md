# Hisky Production — Website (Netlify-ready)

Yeh ek complete, ready-to-deploy website hai. Isme 2 "backend" cheezein already connected hain:

1. **Contact form → Netlify Forms** — koi extra server code nahi chahiye. Submissions seedha aapke Netlify dashboard mein aayengi (aur waha se email notification bhi laga sakte hain).
2. **Portfolio / Case Studies / Testimonials / Team / Blog / Stats → Admin Panel (`/admin`)** — yeh sab ab code mein nahi, `data/*.json` files mein hain. Ek free CMS (Decap CMS) admin panel se aap in files ko browser se edit kar sakte hain — bina code chhue, bina developer ke.

Yeh site plain HTML + React (CDN se) hai — **no build step required**. Netlify par bilkul as-is deploy ho jayegi.

---

## 1. Deploy karne ka sahi tareeka

Admin panel (backend) kaam kare, isliye site ko **GitHub repo ke through** deploy karna zaroori hai (sirf drag-and-drop se admin panel kaam nahi karega, kyunki usko changes commit karne ke liye ek real git repo chahiye).

**Steps:**

1. Is poore folder ko GitHub par ek naye repository mein push kar dein.
2. [Netlify](https://app.netlify.com) par jaake **"Add new site" → "Import an existing project"** choose karein, apna GitHub repo select karein.
3. Build settings yeh rahenge (already `netlify.toml` mein set hain):
   - Build command: *(khali chhod dein)*
   - Publish directory: `.`
4. **Deploy site** par click karein. Kuch second mein aapki site live ho jayegi (e.g. `yourname.netlify.app`).

---

## 2. Contact form ko activate karna

Deploy hone ke baad Netlify khud form ko detect kar lega (form ka naam `contact` hai). Kuch extra karne ki zaroorat nahi.

- Submissions dekhne ke liye: Netlify dashboard → aapki site → **Forms** tab.
- Email notification chahiye toh: Forms tab → **Settings and usage** → **Add notification** → apna email add karein.

---

## 3. Admin panel (portfolio/blog editing) activate karna

Admin panel ke peeche Netlify Identity + Git Gateway hai — yeh bhi free hai.

1. Netlify dashboard → aapki site → **Site configuration → Identity** → **Enable Identity**.
2. Wahi page par, **Registration preferences** mein "Invite only" select kar dein (taaki koi bhi random user sign up na kar sake).
3. **Services → Git Gateway** → **Enable Git Gateway**.
4. **Identity → Invite users** → apna email daal kar khud ko invite karein. Aapko ek email milega — usse password set karein.
5. Ab apni site kholein: `yourname.netlify.app/admin`. Login karein.
6. Yahin se aap **Site Settings (logo, homepage headline/subheading, footer text, stats, contact info), Portfolio Projects, Twitch & YouTube Graphics (image upload ke saath), Case Studies, Testimonials, Team, Blog Posts** — sab edit/add/delete kar sakte hain, images upload kar sakte hain. Save karte hi site apne aap rebuild hoke update ho jaayegi (1-2 minute mein).

---

## Folder structure

```
index.html              → poori website (React SPA, hash-routing: #/services, #/portfolio, etc.)
netlify.toml             → Netlify config
admin/
  index.html             → CMS admin panel entry point
  config.yml             → CMS collections (yahan se fields add/remove kiye ja sakte hain)
data/
  projects.json          → portfolio items
  creator-graphics.json  → Twitch/YouTube graphics portfolio (image upload)
  case-studies.json      → case studies
  testimonials.json      → testimonials
  team.json               → team members
  blog-posts.json         → blog articles
  settings.json           → brand (logo, headline, subheading, footer text), stats, contact info
assets/
  uploads/               → CMS se upload ki gayi images yahan save hongi
```

## Content directly code mein edit karna (bina CMS ke)

Agar CMS use nahi karna, toh `data/` folder ki JSON files ko seedha GitHub par edit kar sakte hain (ya local mein edit karke push kar dein) — dono tareeke kaam karte hain, dono se site update ho jaati hai.

**Services aur Twitch/YouTube Graphics categories** abhi bhi `index.html` ke andar hardcoded hain (kyunki yeh fixed 7+8 items hain, baar baar change nahi hote). Agar inhe bhi CMS se editable banwana ho, bata dein — add kar dunga.

## Local mein test karna

`index.html` ko seedha browser mein khol kar bhi dekh sakte hain — lekin `data/*.json` files fetch nahi hongi (browser security ki wajah se local file se fetch block hota hai), toh placeholder content dikhega. Real content dekhne ke liye ek simple local server chalayein:

```
cd site
python3 -m http.server 8000
```

Phir browser mein `http://localhost:8000` kholein — ab JSON data bhi load hoga.
