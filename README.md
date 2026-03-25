# Intona

Browser-based erhu practice coach.

Listens to your playing through the microphone, tracks your posture and hand position via webcam, and gives real-time coaching feedback. No server, no account, no data sent anywhere. Everything runs locally in Chrome.

---

## Run locally

```
cd intona
python3 -m http.server 8000
```

Open Chrome at `http://localhost:8000`. File:// won't work — Chrome requires localhost for camera and mic.

---

## Requirements

- Chrome (required — Safari and Firefox lack the WebAssembly/WebGL support needed for pose detection)
- Webcam + microphone
- Internet on first load (~10MB of MediaPipe models downloaded from CDN, then cached)

---

## Tech

- Single HTML file, no build step
- [Pitchy](https://github.com/ianprime0509/pitchy) — McLeod pitch detection
- [MediaPipe Tasks Vision](https://ai.google.dev/edge/mediapipe/solutions/vision/pose_landmarker) — pose and hand tracking
- Web Audio API — microphone input, pitch analysis
- `notes.json` — curriculum data (notes, drills, coaching messages)

---

## Deploy

Static site — works on Vercel with no configuration.

1. Push to GitHub
2. Import repo at vercel.com
3. Framework preset: **Other** (static)
4. Deploy

---

## Notes

- Bow scoring is intentionally disabled — scoring bow angle without calibrated reference data would produce more false corrections than useful feedback.
- The app degrades gracefully if pose detection fails (audio-only coaching continues).
- Tested on macOS Chrome. Other platforms may work but are not tested.
