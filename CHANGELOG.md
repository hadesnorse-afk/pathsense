# Changelog

## v2.1
- One message at a time, with a 2.5 s quiet gap after each, so alerts never run back to back.
- Only obstacles in the walking path are announced; things to the side only when they're about to touch you.
- "Stop" warnings cut in instantly, even mid-sentence.
- Go-around advice only comes with "Stop".

## v2.0
- Hands-free: the camera turns on by itself, and one tap anywhere turns on the voice.
- Gestures: tap anywhere to hear what's around you, press and hold to pause, shake to repeat.
- Resumes on its own after switching apps.
- Distance in metres or steps, estimated from each object's typical height.
- Remembers an obstacle that vanishes while close (it usually filled the camera view) and keeps warning.
- Parking-sensor beeps that speed up as the nearest thing ahead gets closer.
- TVs, tables, fridges and other furniture count as obstacles.

## v1.3
- Removed "Path clear". The model can't see doors, walls, poles or steps, so it can't promise a clear path. The app now says so on the start screen.

## v1.2
- Added a WebAssembly backend between WebGL and CPU, for phones where Safari's WebGL fails.
- Shows the reason WebGL failed.

## v1.1
- Loading shows its stage and elapsed time, and announces "Vision model ready" when done.
- Falls back to CPU if WebGL fails or hangs.

## v1.0
- First version: back camera, COCO-SSD obstacle detection, spoken alerts with left/ahead/right or clock directions, screen reader mode, settings and a test session log.
