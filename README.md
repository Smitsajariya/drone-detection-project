# Vision-Based Real-Time Drone Detection

**Smit Sakariya**

A software-only system that detects drones in a video feed and reports where they are, frame by frame. It's built as a virtual simulator plus a working detector. There's no RF hardware and no jamming, so the whole project stays safe, legal and reproducible on a laptop.

## Demos

| File | What it does |
|---|---|
| [`Interceptor-Grid.html`](Interceptor-Grid.html) | 3D simulation (Three.js) of a flying interceptor drone that detects, tracks, locks onto and neutralizes a target drone: the full sense → track → engage loop. |
| [`Drone-Vision-Detector.html`](Drone-Vision-Detector.html) | Live video detector. It finds the drone in a sky clip by motion and shape and draws a YOLO-style box with a confidence score and a telemetry panel. A sample clip is built in, and **Upload clip** lets you try your own drone video. |

**Run:** download the repo and open either `.html` file in any modern browser. No install is needed.

## Roadmap

1. A detection engine that finds drones in a live camera or video stream and draws labelled boxes with confidence.
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
