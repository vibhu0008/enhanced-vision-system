# Raw YOLO Baseline Results

## Test
Raw YOLO object detection on the initial low-light sample video.

## Status
Completed successfully on 2026-08-23.

## Environment
- Python: 3.11.6
- Virtual environment: project-local `venv`
- Model: YOLOv8n (`yolov8n.pt`)
- Input: `data/raw_video/lowlight.mp4`
- Output CSV: `results/raw_lowlight.csv`

## Observed Results
- Total detection rows: **30**
- Detected classes:
  - car: 15
  - tv: 7
  - person: 4
  - bird: 3
  - bear: 1
- Average confidence across recorded detections: **0.389**
- Average inference FPS reported by the current script: **18.56 FPS**
- Annotated frames were generated successfully at 30-frame intervals through at least frame 780.

## Interpretation
The baseline pipeline is operational: the video was processed without errors and YOLO produced detections.

The initial detection confidence is relatively low (0.389 average), which makes this a useful baseline for testing whether enhancement improves detection quality. However, these results alone do **not** establish that enhancement will improve detection or that the system is real-time.

## Important Measurement Limitation
The current script stores the same per-inference FPS value once for each detected bounding box. Therefore, `18.56 FPS` is an **average of recorded per-inference FPS values for frames that produced detections**, not a complete end-to-end video throughput measurement. It should not be treated as the final real-time FPS result.

For the final live-camera feasibility test, measure:
- total end-to-end FPS
- frame processing latency
- enhancement time per frame
- YOLO inference time per frame
- display/capture overhead
- dropped frames

## Next Experiment
Run the exact same `lowlight.mp4` through the enhancement pipeline (gamma correction + CLAHE + bilateral denoising) and compare detection quality and processing cost against this baseline.
