# Project State

## Project
Enhanced Vision System

## Current Stage
Feasibility validation — raw vs enhanced video object detection

## Completed
- Repository created
- Shared AI collaboration rules created in `COLLABORATION.md`
- Claude GitHub integration connected to the project
- ChatGPT GitHub access connected
- Initial raw-vs-enhanced YOLO feasibility test plan added as `REALTIME_VIDEO_ENHANCEMENT_TEST.md`

## Currently Working On
Determine whether classical image enhancement can improve object detection on degraded video while maintaining practical processing performance for a future real-time pipeline.

## Test Approach
- Establish a raw-video YOLO baseline.
- Apply a classical enhancement pipeline using gamma correction + CLAHE + bilateral denoising.
- Run YOLO on the enhanced video.
- Compare detection count and confidence between raw and enhanced video.
- Repeat under multiple conditions such as normal lighting, low light, fog, dust, and different distances.
- Treat live-camera end-to-end FPS and latency as the final real-time validation rather than assuming inference FPS alone proves real-time capability.

## Decisions
- Use a sample-video feasibility test before attempting a live real-time implementation.
- Keep raw and enhanced runs on the same source video for comparison.
- Record results in CSV files and compare them quantitatively.
- Use the GitHub repository as the shared source of truth for ChatGPT and Claude.

## Problems
No experimental results yet. Feasibility has not been established.

## Next Tasks
1. Set up the Python/VS Code environment.
2. Add 1–2 short sample videos, including a low-light clip.
3. Run the raw YOLO baseline.
4. Run the enhanced pipeline.
5. Compare detection and confidence results.
6. Measure processing performance.
7. Repeat across additional conditions.
8. Decide whether to proceed to live-camera testing based on measured evidence.

## Test Guide
See `REALTIME_VIDEO_ENHANCEMENT_TEST.md` for the current step-by-step feasibility procedure.

## Last Updated
2026-08-22
