# Track — Tasks & Habits

A lightweight, mobile-first web app for tracking daily **habits** and **tasks**.
Single self-contained `index.html` — no build step, no server. Data is saved in the browser via `localStorage`.

## Features

### Habits
- Daily sheet with a scrollable day strip; switch between months.
- One combined month grid (habits as rows, days as columns) with a frozen habit column.
- Current & longest streaks, optional weekly goals, month completion %.

### Tasks
- Month calendar (weeks start **Sunday**) with per-day task planning.
- Week planner to zoom into a single week.
- Task priorities (High / Med / Low) and one-tap carry-over of unfinished past tasks.

### General
- Light / dark theme, mobile-friendly, works offline.
- Optional **cloud sync** across devices via Firebase (one personal account).

## Usage
Open `index.html` in any browser. Without any setup it runs fully local (data stays in that browser).

## Cloud sync setup (optional, ~5 min, free)

Sync your habits & tasks across devices with a single account. Firebase's free
Spark tier is far more than enough for personal use (no card required).

1. Go to <https://console.firebase.google.com> → **Add project** (e.g. `habit-tracker`). Skip Analytics.
2. **Build → Firestore Database → Create database** → *Production mode*.
3. **Build → Authentication → Get started → Email/Password → Enable.**
4. **Project settings (⚙️) → Your apps → Web (`</>`)** → register the app → copy the `firebaseConfig` values.
5. In `index.html`, find the `firebaseConfig` block near the top of the `<script>` and paste your values over the `PASTE_…` placeholders.
6. Set the Firestore **security rules** (Firestore → Rules) so only *you* can read/write *your* data:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /users/{uid} {
         allow read, write: if request.auth != null && request.auth.uid == uid;
       }
     }
   }
   ```

7. Open the app, tap **Create your account** the first time, then **Sign in** with the same email on every other device.

Notes:
- The `firebaseConfig` values are safe to keep in the file — they're identifiers, not secrets. The rules above are what actually protect your data.
- "Face ID / Touch ID" login = let your browser/OS save the password and autofill it biometrically on repeat visits.
- Sync is automatic and two-way; the ☁️ button in the header shows sync status and lets you sign out. Offline changes sync when you reconnect.
