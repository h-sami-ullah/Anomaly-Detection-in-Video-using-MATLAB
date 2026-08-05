# Anomaly Detection in Video using MATLAB

Anomalous behavior detection in surveillance video combining optical flow,
a social force model, and Particle Swarm Optimization (PSO). Runs on the
UMN dataset (umndataset.avi).

## How it works

1. **Optical flow** — Lucas-Kanade optical flow is computed on every frame,
   producing per-pixel velocity vectors (`Vx`, `Vy`).
2. **Social force model** — the interaction/force field is derived from the
   deviation between the instantaneous flow and its temporal average, which
   grows when people behave erratically (the prelude to a crowd panic).
3. **PSO search** — a swarm of particles searches the force field for the
   region of maximum anomalous force each frame, tracking the suspicious
   location via particle velocity/position updates.
4. **Detection** — the frame's total anomalous force is compared against a
   reference frame; when it exceeds a threshold for several consecutive
   frames, the frame is flagged `Abnormal` (with an audible alarm).

## Repository structure

```
Anomaly-Detection-in-Video-using-MATLAB/
├── anomaly_detection_in_video.m   Main pipeline (optical flow + PSO + detection)
├── umndataset.avi                 UMN dataset clip (320x240, 306 frames)
├── rehma6-3059519-large.gif       Anomaly demonstration animation
└── README.md
```

## Getting started

1. Ensure `umndataset.avi` is in the same folder as the script (it is
   included in this repository).
2. Open MATLAB and run:

```matlab
>> cd 'path/to/Anomaly-Detection-in-Video-using-MATLAB'
>> anomaly_detection_in_video
```

The output window shows each frame labeled `Normal` or `Abnormal`, with a
warning sound when abnormal behavior is detected.

## Requirements

- MATLAB with the Computer Vision Toolbox (`vision.VideoFileReader`,
  `opticalFlowLK`, `insertText`).
- `umndataset.avi` (UMN dataset) — 320x240 resolution, 306 frames, which
  the PSO stage assumes (see `fintxy = zeros(100, 2, 306)`).

## Notes

- Parameters such as the optical-flow noise threshold (`0.0035`), PSO
  constants (`c1`, `c2`), and the anomaly threshold (`40`) were tuned for
  the UMN dataset and may need adjustment for other videos.
- The swarm uses 100 particles over up to 100 iterations per frame; the
  runtime scales with the number of frames.
