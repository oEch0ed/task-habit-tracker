# Ear — train your ears

A Duolingo-style **ear trainer for guitarists**. Bite-sized daily lessons, pure
listen-and-answer — you never touch the guitar. Single self-contained
`index.html`: no build step, no server, no audio files (every sound is
synthesised live with the Web Audio API).

Live: <https://oech0ed.github.io/task-habit-tracker/>

## The path

Four units, 36 lessons. Each lesson is 10 questions; score 70%+ to earn a star
and unlock the next one (85% = 2 stars, 100% = 3 and a 👑).

| Unit | What you do |
|---|---|
| 📏 **Intervals** | Hear two notes — name the interval. Ascending → descending → harmonic → all mixed. Each miss shows the classic song hook for that interval. |
| 🎹 **Chords by ear** | Major vs minor → diminished → augmented → block voicings → inversions → the four seventh chords. |
| 🎼 **Scales & Modes** | Hear a scale run up and down — name it. Major/minor, pentatonics, blues, then all seven modes. |
| 🎸 **Fretboard** | Silent. Read an SVG diagram: name the note, then name the triad shape on each string set, then the whole chord. |

## The game layer

- **XP** (2 per correct answer, +10 for passing a lesson), **daily goal** ring, **🔥 day streak**.
- Instant feedback: the right answer is highlighted, the correct sound replays, and the notes are named.
- **Practice tab** — free sessions on anything unlocked, plus a mixed review; no unlocking, lower XP.
- **Stats tab** — accuracy per interval / chord / scale / note, and a "weak spots" drill.
- **Adaptive weighting**: items you keep missing come up more often, both inside a lesson and in mixed review.

## Cloud sync

Optional, one personal account (Firebase Auth + Firestore, compat SDK via CDN).
Progress is stored under `users/{uid}.ear` and written with `merge: true`.
Without signing in the app runs fully local (localStorage).

Firestore rules:

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

The `firebaseConfig` values in `index.html` are identifiers, not secrets — the
rules above are what protect the data.

## Development

Open `index.html` in any browser. Tests use jsdom (stub `AudioContext`, set
`url: 'http://localhost/'` so `localStorage` works); they generate every
question type a few hundred times and assert the audio matches its label.

## History

This repo previously held **Track**, a habits & tasks app. It was replaced by
the ear trainer; the old code is in the git history.
