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

## Currently Working On
Determine whether classical image enhancement can improve object detection on degraded video while maintaining practical processing performance for a future real-time pipeline.

## Test Approach
- Establish a raw-video YOLO baseline.
- Apply a classical enhancement pipeline using gamma correction + CLAHE + bilateral denoising.
- Run YOLO on the enhanced video.
- Compare detection quality and processing cost between raw and enhanced video.
- Repeat under multiple conditions such as normal lighting, low light, fog, dust, and different distances.
- Treat live-camera end-to-end FPS and latency as the final real-time validation rather than assuming inference FPS alone proves real-time capability.

## Current Baseline
- Total detection rows: 30
- Classes: car 15, tv 7, person 4, bird 3, bear 1
- Average confidence: 0.389
- Script-reported average inference FPS: 18.56 FPS
- The baseline video processing completed without errors and generated annotated frames.
- The 18.56 FPS figure is not the final end-to-end real-time FPS because the current script records FPS only for frames with detections and repeats that value per detection row.

## Decisions
- Use a sample-video feasibility test before attempting a live real-time implementation.
- Keep raw and enhanced runs on the same source video for comparison.
- Record results in CSV files and compare them quantitatively.
- Use the GitHub repository as the shared source of truth for ChatGPT and Claude.
- Do not treat the initial baseline as proof of feasibility; it is only the reference condition for the enhancement experiment.

## Problems
- Feasibility has not been established.
- The current baseline script's FPS metric is not a complete end-to-end throughput measurement.
- YOLO produced several unexpected classes (`tv`, `bear`, `bird`) in the initial test, so visual inspection and/or a better-controlled test scene may be useful when evaluating detection quality.

## Next Tasks
1. Create the enhancement module using gamma correction + CLAHE + bilateral denoising.
2. Run the enhanced pipeline on the exact same `lowlight.mp4`.
3. Compare raw vs enhanced detection results and confidence.
4. Record enhancement processing cost.
5. Improve the performance measurement so it represents end-to-end processing throughput.
6. Repeat across additional conditions.
7. Decide whether to proceed to live-camera testing based on measured evidence.

## Test Guide
See `REALTIME_VIDEO_ENHANCEMENT_TEST.md` for the current step-by-step feasibility procedure.
See `BASELINE_RESULTS.md` for the measured raw baseline.

## Last Updated
2026-08-23
