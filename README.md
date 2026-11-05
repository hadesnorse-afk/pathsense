# PathSense

A lightweight phone web app that helps visually impaired people walk around campus. It uses the phone's camera and an on-device vision model to detect obstacles, estimates how far away they are, and speaks them out loud:

> "TV, about 2 metres, ahead. Try going around on your left."
> "Stop. Person, about 1 metre, ahead. Try going around on your right."

PathSense supports a white cane or guide dog. It does not replace them.

## Try it

Open **https://hadesnorse-afk.github.io/pathsense/** on your phone (Safari on iPhone, Chrome on Android). The camera turns on by itself; allow it when asked, then **tap anywhere once** to turn on the voice. Phones block sound from web pages until the screen has been touched, so that one tap is unavoidable.

Phones only allow camera access on `https` links, so opening `index.html` straight from the Files app or an AirDrop will not work.

**On a laptop:** run `python -m http.server 8000` in this folder and open `http://localhost:8000` (localhost counts as secure, so the webcam works).

## Using it without looking

| Gesture | What happens |
|---|---|
| Tap anywhere on the camera view | Hear what's around you, nearest first, with distances |
| Press and hold | Pause or resume |
| Shake the phone | Hear what's around you again |
| Switch away and come back | Camera resumes on its own |

With VoiceOver or TalkBack on, the camera view is one big button: double-tap it to hear what's around you. In Settings, choose **Speak through → My screen reader** so the two voices don't overlap.

## What it does

- Detects people, two-wheelers, bicycles, cars, buses, dogs, cows, benches, chairs, tables, TVs and other everyday obstacles (COCO-SSD, 80 object types, runs fully on the phone with TensorFlow.js).
- Estimates distance from each object's typical size, in metres or steps.
- Says where each one is: left, ahead or right, or clock positions (10 to 2 o'clock).
- Suggests which side to go around, based on what it can see on each side.
- Keeps warning while something stays close: very close obstacles are repeated every 3 seconds, and again whenever they get closer.
- Remembers an obstacle that disappears from view while close (usually because it fills the whole camera) and warns "may be right in front of you".
- Beeps faster as you get closer to the nearest thing ahead, like a car parking sensor.
- Never says the path is clear, because it cannot yet see doors, walls, poles or steps (Step 2 adds these).
- Falls back from WebGL to WebAssembly to plain CPU if the phone's graphics engine fails.
- Coloured boxes with distances for sighted helpers: red = very close, yellow = close, dashed white = further away.
- **Test session log** in Settings: copy every announcement with timestamps as CSV for user-study analysis.

## Tuning

Constants at the top of the script in `index.html`:

| Constant | Default | What it does |
|---|---|---|
| `CAMERA_FOV_DEG` | `70` | Camera field of view along the long side. Raise it if distances read too far, lower it if too near |
| `VERY_CLOSE_M` | `1.2` | Nearer than this: "Stop." ahead, "Careful." on the sides |
| `CLOSE_M` | `2.6` | Nearer than this: announced as an obstacle |
| `REPEAT_MS` | `[15000, 6000, 3000]` | Repeat the same object on the same side after (far, close, very close) |
| `MEMORY_MS` | `3000` | How long to keep warning about an obstacle that vanished while close |
| `LOOP_GAP_MS` | `40` | Pause between detections; raise it if the phone gets hot |

Typical object heights used for distance live in the `CLASSES` table.

## Roadmap

1. **Core app** (done): camera, obstacle detection, distance, spoken directions, hands-free use.
2. **Campus model:** recognise what the stock model can't: doors (open or closed), walls, stairs, poles, e-scooters, construction barriers, crosswalk buttons.
3. **Signs and entrances:** read specific local signs and help find the exact entrance of a building.
4. **User testing:** 5 visually impaired testers on campus with a local association for the blind, using the session log and a feedback form.

## Tech

Single `index.html`, no build step. TensorFlow.js 4.22.0 (WebGL, WebAssembly and CPU backends) and COCO-SSD 2.2.3 from jsDelivr, Web Speech API for voice, Web Audio for beeps, Screen Wake Lock to keep the screen on while guiding.
