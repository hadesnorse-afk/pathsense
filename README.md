# PathSense

A lightweight phone web app that helps visually impaired people walk around campus. It uses the phone's camera and an on-device vision model to detect obstacles and speaks them out loud with a direction and distance:

> "Two-wheeler, close, on your right."
> "Stop. Person, very close, ahead."

PathSense supports a white cane or guide dog. It does not replace them.

## Try it

Open the GitHub Pages link on your phone (Safari on iPhone, Chrome on Android), allow the camera, and tap **Start guide**.

Phones only allow camera access on `https` links, so opening `index.html` straight from the Files app or an AirDrop will not work.

**To turn on the link:** repo **Settings → Pages → Deploy from a branch → `main` / `(root)` → Save**. After about a minute the app is live at `https://hadesnorse-afk.github.io/pathsense/`.

**On a laptop:** run `python -m http.server 8000` in this folder and open `http://localhost:8000` (localhost counts as secure, so the webcam works).

## Features

- Detects people, two-wheelers, bicycles, cars, buses, dogs, cows, benches, chairs and other everyday obstacles (COCO-SSD, 80 object types, runs fully on the phone with TensorFlow.js).
- Says where each one is: left, ahead or right, or clock positions (10 to 2 o'clock).
- Escalates as you get closer: far, close, very close. Urgent alerts interrupt everything else.
- Announces each object once per side instead of repeating every frame. It never says the path is clear, because it cannot yet see doors, walls, poles or steps (Step 2 adds these).
- **What's around me?** button describes the whole scene from left to right.
- Works with TalkBack and VoiceOver (Settings → Speak through → My screen reader).
- Vibrates on alerts (Android only; iPhone Safari has no web vibration).
- Coloured boxes on screen for sighted helpers: red = very close, yellow = close, dashed white = further away.
- **Test session log** in Settings: copy every announcement with timestamps as CSV for user-study analysis.

## Tuning

Constants at the top of the script in `index.html`:

| Constant | Default | What it does |
|---|---|---|
| `CLOSE_AT` | `0.36` | Object size (share of the frame) that counts as "close" |
| `VERY_CLOSE_AT` | `0.62` | Object size that counts as "very close" |
| `REPEAT_MS` | `[12000, 7000, 4000]` | Earliest repeat of the same object on the same side (far, close, very close) |
| `LOOP_GAP_MS` | `60` | Pause between detections; raise it if the phone gets hot |
| `DETECT_LONG_SIDE` | `480` | Frame size used for detection |

Walk a corridor with the app running and adjust `CLOSE_AT` / `VERY_CLOSE_AT` until alerts fire at sensible distances.

## Roadmap

1. **Core app** (done): camera, obstacle detection, spoken directions.
2. **Campus model:** train a custom model on local hazards the stock model does not know: e-scooters, construction barriers, crosswalk buttons, building entrances.
3. **Signs and entrances:** read specific local signs and help find the exact entrance of a building.
4. **User testing:** 5 visually impaired testers on campus with a local association for the blind, using the session log and a feedback form.

## Tech

Single `index.html`, no build step. TensorFlow.js 4.22.0 and COCO-SSD 2.2.3 from jsDelivr, Web Speech API for voice, Screen Wake Lock to keep the screen on while guiding.
