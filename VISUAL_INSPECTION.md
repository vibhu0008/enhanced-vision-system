# Visual Inspection — Raw vs Enhanced Low-Light Frames

## Source
User-provided frame pairs from the initial low-light video test, reviewed on 2026-08-23.

## Observations
- The enhancement pipeline visibly raises brightness in dark regions, especially the road surface and vegetation.
- Road texture and roadside vegetation become more visible in the enhanced frames.
- The enhanced frames also show stronger bright regions around the oncoming headlights, with visible clipping/haloing.
- Enhancement increases visible noise/texture in some dark and foliage regions rather than simply revealing clean detail.
- The visual change is substantial enough to confirm that the enhancement pipeline is doing meaningful image processing.

## Detection Assessment Limitation
- The uploaded images available for this inspection do not show YOLO bounding boxes/labels clearly enough to determine which raw/enhanced detections are correct or false positives.
- Therefore, visual quality improvement can be assessed, but detection-quality improvement cannot yet be established from these images alone.

## Interpretation
The enhancement improves scene visibility to a human viewer in the tested low-light road scene, but it also increases highlight clipping and noise. This supports continuing the experiment, while tuning the enhancement parameters and using annotated detection frames or ground truth to determine whether the improved visibility actually helps object detection.

## Next Step
- Inspect the actual annotated raw and enhanced YOLO frames containing bounding boxes.
- Compare correct detections and false positives for the same frames.
- Separately measure enhancement processing time before deciding on real-time feasibility.
