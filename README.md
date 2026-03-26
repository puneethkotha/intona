# Intona

Real-time practice coach for pitch, timing, and bow control — runs entirely in the browser.

---

## What it does

- Detects pitch from your microphone and gives immediate feedback on intonation
- Tracks timing (attack latency) and bow control via optional camera input
- Guides you through structured sessions: warmup, drills, scales, and performance phases
- Saves sessions, streaks, and skill progress in localStorage
- Supports custom note sequences for piece practice
- Persists calibration so returning users skip setup on repeat visits

---

## How it works

- **Pitch detection:** [Pitchy v4](https://github.com/ianprime0509/pitchy) (McLeod algorithm) via Web Audio API
- **Pose/bow tracking:** [MediaPipe Tasks Vision v0.10.14](https://developers.google.com/mediapipe) — optional, camera only
- **Session engine:** Phase-based state machine (warmup → drill → scale → performance)
- **Drill engine:** WAITING → ATTEMPT → ANALYZING → FEEDBACK_HOLD loop, exits after sufficient clean reps
- **Storage:** Single `localStorage` key (`intona_v1`), JSON blob, schema-versioned
- **No backend. No build step.** One HTML file + one JSON curriculum file.

---

## Running locally

```bash
git clone https://github.com/puneethkotha/intona.git
cd intona
python3 -m http.server 8000
```

Open `http://localhost:8000` in Chrome or Firefox.

> Do not open `index.html` directly as a file. Web Audio and MediaPipe require a server context (even localhost).

---

## Deployment

The app is a static site. Deploy anywhere that serves static files.

**Vercel (recommended)**

Connect the repo to [Vercel](https://vercel.com). No configuration needed beyond the included `vercel.json`.

**GitHub Pages**

Enable Pages on the `main` branch root. Works without modification.

---

## Usage

1. Open the app and grant microphone access when prompted
2. Complete a short calibration (play and hold D4)
3. Follow the on-screen drill prompts — play the note shown, hold it steady
4. Session summary and progress are saved automatically at the end

Calibration is skipped automatically if completed within the last 20 hours.

---

## Custom pieces

Add a piece by pasting a note sequence in the Pieces section on the start screen.

**Note format:** `NOTE[xDURATION][~]` — duration in beats (default 1), `~` marks a tie, `R` for rest.

```json
{
  "title": "Scale fragment",
  "notes": "D4 E4 F#4 G4 A4x2 R G4~ G4 F#4 E4 D4"
}
```

Pieces are stored locally. Each note becomes a hold drill with 2 required clean reps.

---

## Browser support

| Browser | Status |
|---|---|
| Chrome 100+ | Full support |
| Firefox 100+ | Full support |
| Safari 15+ | Supported (camera tracking may be slower) |
| Safari < 15 | Not supported |
| Mobile Chrome | Supported (camera optional) |

Requires HTTPS in production. `localhost` works without it.

---

## Notes

- Camera is optional. The app works fully with microphone only.
- If MediaPipe fails to load, the app continues with audio-only coaching.
- All data is local. Clearing browser storage resets progress.
- The curriculum (`notes.json`) must be served alongside `index.html` — it is not embedded.

---

## License

MIT
