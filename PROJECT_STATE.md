# Project State

## Project
Enhanced Vision System

## Current Stage
Feasibility validation — raw vs enhanced video object detection

## Completed
- Repository created
- Shared AI collaboration rules created in `COLLABORATION.md`
- Claude GitHub integration connected to the project (Claude currently cannot reliably access the repository; user will relay Claude updates when needed)
- ChatGPT GitHub access connected
- Initial raw-vs-enhanced YOLO feasibility test plan added as `REALTIME_VIDEO_ENHANCEMENT_TEST.md`
- Python 3.11.6 project environment created and activated successfully
- Required Python libraries installed
- Initial low-light sample video added
- Raw YOLO baseline test completed successfully
- Raw baseline results recorded in `BASELINE_RESULTS.md`
- Enhancement pipeline implemented in `enhance.py`
- Enhanced YOLO test completed successfully on the same low-light video

## Currently Working On
Determine whether classical image enhancement can improve object detection on degraded video while maintaining practical processing performance for a future real-time pipeline.

## Test Approach
- Establish a raw-video YOLO baseline.
- Apply a classical enhancement pipeline using gamma correction + CLAHE + bilateral denoising.
- Run YOLO on the enhanced video.
- Compare detection quality and processing cost between raw and enhanced video.
- Repeat under multiple conditions such as normal lighting, low light, fog, dust, and different distances.
- Treat live-camera end-to-end FPS and latency as the final real-time validation rather than assuming inference FPS alone proves real-time capability.

## Current Baseline vs Enhanced Results
### Raw
- Total detection rows: 30
- Classes: car 15, tv 7, person 4, bird 3, bear 1
- Average confidence: 0.389
- Script-reported average inference FPS: 18.56 FPS

### Enhanced
- Total detection rows: 47
- Classes: traffic light 17, car 13, bird 9, person 6, tv 2
- Average confidence: 0.358
- Script-reported average inference FPS: 18.37 FPS

### Initial Interpretation
- Enhanced processing produced 47 detection rows versus 30 for raw processing (+56.7%).
- However, average confidence decreased from 0.389 to 0.358 (-0.031, about 8.0% lower).
- The detected class distribution changed substantially, including new `traffic light` detections and fewer `car` detections.
- Therefore, the current result does **not** establish that enhancement improved detection. More detections may include additional false positives.
- Inference FPS was almost unchanged (18.56 vs 18.37), but this is not a complete end-to-end real-time measurement and does not include the enhancement processing cost in the reported FPS.

## Decisions
- Use a sample-video feasibility test before attempting a live real-time implementation.
- Keep raw and enhanced runs on the same source video for comparison.
- Record results in CSV files and compare them quantitatively.
- Use the GitHub repository as the shared source of truth for ChatGPT and Claude.
- Do not treat the initial baseline as proof of feasibility; it is only the reference condition for the enhancement experiment.
- Do not conclude that the current enhancement pipeline improves detection based only on detection count; visual/ground-truth evaluation and proper performance measurement are required.

## Problems
- Feasibility has not been established.
- The current scripts' FPS metric is not a complete end-to-end throughput measurement.
- The enhanced run produced more detections but lower average confidence and a substantially different class distribution.
- YOLO produced several unexpected classes (`tv`, `bear`, `bird`, and in the enhanced run `traffic light`), so visual inspection and a controlled test scene/ground truth are needed before judging detection improvement.

## Next Tasks
1. Inspect representative raw and enhanced annotated frames side-by-side.
2. Determine which detections are correct vs false positives using the actual video content.
3. Improve the comparison metrics so they measure meaningful detection quality (for example, per-class correct detections/false positives where ground truth is available).
4. Measure enhancement processing time separately from YOLO inference time.
5. Improve the performance measurement so it represents end-to-end processing throughput.
6. Repeat across additional conditions.
7. Decide whether to proceed to live-camera testing based on measured evidence.

## Test Guide
See `REALTIME_VIDEO_ENHANCEMENT_TEST.md` for the current step-by-step feasibility procedure.
See `BASELINE_RESULTS.md` for the measured raw baseline.

## Last Updated
2026-08-23
