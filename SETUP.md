# FitDuo — Tawanda & Nathan fitness tracker

A single-file web app to track, share and compare your training. Tracks all your goals:

- **5×/week** workouts of **≥40 min** HIIT / running / resistance (sessions that qualify are flagged ✓)
- **3×/week** OMAD (one meal a day) — Month 1 any meal, Month 2 prompts for meat + veggies + healthy fat
- **2×/week** water fasting
- **2 L water every day**
- Watch stats (steps, active calories, heart rate, sleep) — manual entry **or** Samsung Health import
- **Compare** view: you vs Nathan, side by side, with a weekly "who's ahead" verdict

---

## Using it right now (local, offline)

Just open `index.html` in your phone/computer browser. It works fully offline and saves to that
device. Add it to your home screen (browser menu → "Add to Home Screen") to use it like an app.

> Local mode keeps data on one device only. For live sharing with Nathan, set up cloud sync below.

---

## Cloud sync (so you and Nathan see each other live)

Cloud sync needs the app served from a web address (Firebase login won't work from a `file://` page).
Easiest free option:

1. Go to **app.netlify.com/drop** and drag the whole `fitness-tracker` folder in. You get a URL like
   `https://your-name.netlify.app`. Both of you open that URL on your phones and "Add to Home Screen".
   *(Firebase Hosting or GitHub Pages work too.)*

Then set up the shared database (one of you does this, then shares the config with the other):

1. **console.firebase.google.com** → **Add project** (any name, skip Analytics).
2. Click the **`</>` (Web)** icon → register an app → copy the whole `firebaseConfig = { ... }` block.
3. **Build → Authentication → Get started → Anonymous → Enable.**
4. **Build → Firestore Database → Create database** (Production mode, any region).
5. Firestore **Rules** tab → paste this → **Publish**:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{db}/documents {
       match /groups/{g}/{rest=**} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```

6. In the app: **Settings → Cloud sync** → paste the config → **Save & connect**.

**Both of you** paste the **same** config and use the **same group code** (e.g. `ironduo2026`).
Each picks their own name. That's it — you'll see each other's progress update live.

---

## Samsung Health import

1. Samsung Health app → **Settings → Download personal data** → you get a ZIP. Unzip it.
2. App → **Watch** tab → **Choose CSV files** → pick the `..._step_daily_trend_...csv` and
   `..._exercise_...csv` files.
3. Steps/calories merge into each day; exercise sessions become workouts. Workout *type* from Samsung
   is a best guess — open the day on the **Today** tab to correct it if needed.

Other files in the export are skipped safely.

---

## Backup

**Settings → Export my data** saves a JSON backup. **Import a backup file** restores it. Handy before
switching devices or just to be safe.

---

### Files

- `index.html` — the entire app (HTML + CSS + JS, no build step)
- `server.js` — tiny local server for previewing (`node server.js` → http://localhost:8765)
- `SETUP.md` — this file
