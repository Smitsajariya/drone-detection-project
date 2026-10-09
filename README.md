# Vision-Based Real-Time Drone Detection

**Smit Sakariya**

A software-only system that detects drones in a video feed and reports where they are, frame by frame. It's built as a virtual simulator plus a working detector. There's no RF hardware and no jamming, so the whole project stays safe, legal and reproducible on a laptop.

## Demos

| File | What it does |
|---|---|
| [`Interceptor-Grid.html`](Interceptor-Grid.html) | 3D simulation (Three.js) of a flying interceptor drone that detects, tracks, locks onto and neutralizes a target drone: the full sense → track → engage loop. |
| [`Drone-Vision-Detector.html`](Drone-Vision-Detector.html) | Live video detector. It finds the drone in a sky clip by motion and shape and draws a YOLO-style box with a confidence score and a telemetry panel. A sample clip is built in, **Upload clip** lets you try your own drone video, and **● Live camera** runs detection in real time on a webcam or phone camera. |

**Run:** download the repo and open either `.html` file in any modern browser. No install is needed.

## Live camera mode

1. Open `Drone-Vision-Detector.html` in Chrome or Edge (or host it over https to use a phone) and press **● Live camera**, then allow camera access. Phones use the back camera.
2. Keep the camera **still** (tripod or a fixed surface) and point it at the sky. Auto-detect looks for small *moving* targets, so a shaking camera gives false alarms.
3. Settings:
   - **Detail**: processing resolution. Use *max* for small, far-away drones.
   - **Target**: *dark on sky* for daytime, *bright* for night or drone LEDs.
   - **Sensitivity**: lower it if birds or leaves trigger detections.
   - **Alert sound**, **Snapshot** (saves the frame with the box as a PNG), and a live fps readout.
4. **Click-track** also works on the live feed: click the drone to lock the box by hand.

Tested with a simulated camera feed (moving drone, drifting clouds, sensor noise): it locks in about 1 s and holds the box within about 4 px at 30 fps. The detector uses classic computer vision (motion + local contrast + velocity tracking), not a neural network, so it can't yet tell a drone from a bird. That's the next step in the roadmap.

Camera frames are processed locally in the browser and never uploaded.

## Roadmap

1. ~~A detection engine that finds drones in a live camera or video stream~~ (done, classic CV); next, a drone-trained YOLO model to separate drones from birds.
2. Multi-frame tracking so each drone keeps a stable identity as it moves.
3. Performance measurement: precision, recall, mAP, frame rate, and the drone-vs-bird false-alarm rate.

## Planned pipeline

```
Camera / video  →  YOLO (drone-trained)  →  ByteTrack / DeepSORT  →  UI (boxes + telemetry)  →  Metrics
```

| Component | Approach | Tool |
|---|---|---|
| Detection core | Fine-tune YOLOv8 on a drone dataset | Python · Ultralytics |
| Live input | Webcam, video file or network stream | OpenCV |
| Tracking | Keep drone identity across frames | ByteTrack / DeepSORT |
| Interface | Live boxes, confidence, telemetry | Browser UI (built) |
| Evaluation | Benchmark on labelled test videos | DUT Anti-UAV, Drone-vs-Bird |

## Note

The browser demos show the concept. No radio frequency is emitted and no drone is interfered with at any stage. This project is detection only.
