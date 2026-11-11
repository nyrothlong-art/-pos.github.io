# ម៉ាតភ្នំទូកមាស POS — Frontend + Backend

```
docs/     Frontend (GitHub Pages)  — index.html, api.js, config.js, sw.js, manifest
server/   Backend (Node.js + Express + SQLite) — server.js
```

## ស្ថាបត្យកម្ម
- **Frontend** ដំណើរការលើ GitHub Pages (Static) ហើយអាច Install លើទូរស័ព្ទ និងបើក Offline បាន។
- **Backend** (GitHub Pages ដំណើរការ Server មិនបាន ត្រូវដាក់ដាច់ដោយឡែក) ទទួលខុសត្រូវ៖
  Login (JWT + ពាក្យសម្ងាត់ Hash), រក្សាទិន្នន័យ, សិទ្ធិតាម Role, ផ្ញើ Telegram + Gmail។
- Offline: កម្មវិធីរក្សាទុកក្នុង Browser សិន រួច Sync ទៅ Server ដោយស្វ័យប្រវត្តិពេលមានអ៊ីនធឺណិត
  (ការលក់ និងសារ Notification ដែលមិនទាន់ផ្ញើ ត្រូវរង់ចាំក្នុង Queue)។

## ១) Backend
### សាកល្បងលើកុំព្យូទ័រ
```bash
cd server
cp .env.example .env      # កែ JWT_SECRET, ADMIN_PASSWORD
npm install
npm start                 # http://localhost:3000
```
### ដាក់លើ Server (Render / Railway / Fly.io / VPS)
- Root Directory: `server`   Build: `npm install`   Start: `npm start`
- ដាក់ Environment Variables តាម `server/.env.example`
  (`ALLOWED_ORIGINS=https://USERNAME.github.io`)
- **សំខាន់:** ត្រូវភ្ជាប់ Disk/Volume ថេរ ហើយកំណត់ `DB_PATH` ទៅលើវា (ឧ. `/var/data/pos.db`)។
  បើគ្មាន Disk ថេរ ទិន្នន័យនឹងបាត់ពេល Server Restart/Deploy ថ្មី។ `render.yaml` មានឧទាហរណ៍។

### Gmail
បើក 2-Step Verification ក្នុង Google Account → បង្កើត **App Password** → ដាក់ក្នុង `GMAIL_APP_PASSWORD`។

## ២) Frontend (GitHub Pages)
1. បើក `docs/config.js` រួចដាក់ URL Backend៖ `window.POS_API_URL = 'https://pos-api.onrender.com';`
2. Push ទៅ GitHub → **Settings → Pages** → Branch `main` / Folder **`/docs`** → Save។
3. បើក `https://USERNAME.github.io/REPO/` ហើយ Login ជា `Admin` ជាមួយ `ADMIN_PASSWORD`។

## សិទ្ធិតាម Role (Server ពិនិត្យ)
| Role | សិទ្ធិ |
|---|---|
| ADMIN | គ្រប់យ៉ាង (បុគ្គលិក, ចំណាយ, ការកំណត់, លុបវិក្កយបត្រ) |
| CASHIER | លក់, កត់ត្រាលុយអាប់; កែបានតែចំនួនស្តុក (qty) ពេលលក់; មិនអាចកែតម្លៃ/លុបទំនិញ/លុបវិក្កយបត្រ |
| STOCK | គ្រប់គ្រងទំនិញក្នុងស្តុក |

## ចំណាំ
- ទំនិញគំរូ 3 មុខ (Bacchus ...) នឹងត្រូវផ្ញើទៅ Server នៅពេល Admin Login លើកដំបូង — លុបវាចេញបាន។
- Sync គឺ Polling រៀងរាល់ ~20 វិនាទី (មិនមែន Realtime ភ្លាមៗ)។ បើពីរម៉ាស៊ីនកែទំនិញតែមួយក្នុងពេលជាមួយគ្នា ការកែចុងក្រោយឈ្នះ។
- Telegram Token / Gmail Password នៅលើ Server ប៉ុណ្ណោះ មិនបង្ហាញក្នុង Browser ទេ។
- បើ `POS_API_URL` ទទេ កម្មវិធីដំណើរការ Local ម្នាក់ឯងដូចមុន (គ្មាន Backend)។
- ការទូទាត់ KHQR/Bakong នៅមិនទាន់ភ្ជាប់ Backend (មិនទាន់មានក្នុងកូដដើម)។
