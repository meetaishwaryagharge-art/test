# Dr. Smita Karpe – Website + Firebase Dashboard

## File structure
```
index.html              Landing page (consult + journey popups)
journy.html             Heart & Lifestyle Health Check (opens in popup)
Code.gs                 Old Google Sheet backend (optional, keep PRIVATE)
firebase.json           Hosting + Firestore settings
firestore.rules         Security rules (visitors create only, admins read)
.firebaserc             Your Firebase project id
js/
  firebase-config.js    <-- PASTE YOUR FIREBASE KEYS HERE (only file you edit)
  firebase.js           Starts Firebase (shared by site + dashboard)
  site-save.js          Saves appointments + health checks to Firestore
admin/
  login.html            Staff login
  index.html            Dashboard shell (sidebar, topbar)
  css/admin.css         Dashboard design
  js/app.js             Auth guard + router
  js/ui.js              Shared helpers (escape, dates, CSV)
  js/modules/           One file per dashboard section
    index.js            Module registry
    overview.js · appointments.js · healthchecks.js
```

## Setup (about 10 minutes)
1. **Create project:** https://console.firebase.google.com > Add project.
2. **Web app:** Project settings > Your apps > `</>` > register. Copy the config into `js/firebase-config.js`.
3. **Authentication:** Build > Authentication > Get started > enable **Email/Password**. Under Users, **Add user** (your clinic email + password).
4. **Firestore:** Build > Firestore Database > Create database (production mode, region `asia-south1` Mumbai).
5. **Make yourself admin:** In Firestore create collection `admins`, with **Document ID = your user's UID** (copy it from Authentication > Users). Add any field, e.g. `role: "admin"`.
6. **Rules + hosting:**
   ```
   npm i -g firebase-tools
   firebase login
   # put your project id in .firebaserc, then:
   firebase deploy
   ```
   (or paste `firestore.rules` into Firestore > Rules > Publish).
7. **Authorised domains:** Authentication > Settings > Authorised domains – add your own domain if you host elsewhere.
8. Open `https://YOUR-SITE/admin/login.html`.

> Pages use ES modules, so open them through a web server (Firebase Hosting, or `npx serve .` locally), not by double-clicking.

## Add a new dashboard module
Create `admin/js/modules/patients.js` exporting `{ id, title, icon, mount(root) }`, then add it to `modules/index.js`. The sidebar and routing update automatically. Add matching rules to `firestore.rules`.

## Security notes
- Staff access = a user in Authentication **and** a document in `admins/<uid>`. Anyone else is signed out.
- Visitors can only create appointments/health checks with validated fields; they cannot read anything.
- Change the old `ADMIN_PASS` in `Code.gs` if you keep using it.
