# AI Collaboration Rules

## Purpose
This file defines how ChatGPT and Claude should collaborate on the Enhanced Vision System project while using this GitHub repository as the shared source of truth.

## 1. Single Source of Truth
- GitHub is the canonical project state.
- Before doing substantial project work, read `PROJECT_STATE.md`, `TODO.md`, and `DECISIONS.md` when relevant.
- Do not assume that an AI's previous conversation contains the latest project state.

## 2. Keep Project State Updated
- Update `PROJECT_STATE.md` when the current stage, implementation status, blockers, architecture, or major progress changes.
- Update `TODO.md` when tasks are added, completed, blocked, or reprioritized.
- Update `DECISIONS.md` whenever a significant technical, design, tooling, or project-management decision is made.

## 3. Preserve Other AI's Work
- Always inspect the latest version of a file before replacing it.
- Do not overwrite another AI's changes blindly.
- Preserve useful existing content unless there is a clear reason to change it.
- When two approaches conflict, document the conflict and the chosen resolution in `DECISIONS.md`.

## 4. Record Changes Clearly
After making project changes, report:
- What was changed.
- Which files were changed.
- Why the change was made.
- Any tests or validation performed.
- Any remaining issue or next step.

## 5. Engineering Quality
- Prefer technically justified solutions over assumptions.
- State important assumptions explicitly.
- Do not mark a task complete without appropriate verification.
- For simulations, experiments, and measurements, record relevant conditions, parameters, and results.

## 6. Task Ownership
- ChatGPT and Claude may work on the same project independently.
- Neither AI is considered the permanent owner of a task unless explicitly assigned.
- Before starting work on an existing task, check `TODO.md` and the latest project state.

## 7. Git and Change Safety
- Use clear, descriptive commit messages.
- Avoid force-pushing or destructive changes unless explicitly requested by the user.
- Prefer small, understandable commits for major changes.
- Never commit secrets, passwords, API keys, private tokens, or other credentials.

## 8. Synchronization
- GitHub changes are the mechanism for sharing project updates between ChatGPT and Claude.
- Claude should sync its GitHub project knowledge when repository content has changed.
- ChatGPT should read the latest repository files before relying on project state.
- "Real-time" means both AIs can work from the latest shared GitHub state; it does not mean one AI automatically sees the other's conversation in real time.

## 9. Communication With the User
- Do not claim that another AI has performed an action unless the repository state or an explicit user message confirms it.
- If information is missing or stale, say so and check the shared repository when possible.
- Keep the user informed when a change affects architecture, requirements, safety, cost, performance, or project scope.

## 10. Standard Project Files
- `README.md` — project overview and setup instructions.
- `PROJECT_STATE.md` — current state, progress, blockers, and next steps.
- `TODO.md` — actionable task list.
- `DECISIONS.md` — important decisions and their reasoning.
- `COLLABORATION.md` — rules governing AI collaboration.

## Collaboration Principle
Treat the repository as a shared engineering workspace. Read before changing, verify before claiming completion, document important decisions, and leave the project in a state that the other AI can continue from without needing the previous conversation.