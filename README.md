# INFO5995 Assignment 1 — Working Repository

This is the team's main working repository for INFO5995 Assignment 1.

## Current Status

| Task | Status | Branch |
|------|--------|--------|
| Task 1 — Decompile APK | ✅ Done | `joey-task1` |
| Task 2 — System Model | ✅ Done | `joey-task2` |
| Task 3 — Vulnerability Discovery | ✅ Done → Merged to `main` | `joey-task3` |
| Task 4 — Threat Model | ✅ Done | `joey-task4` |
| Task 5 — Report & Presentation | ⏳ Pending | — |

## Branch Structure

| Branch | Purpose |
|--------|---------|
| `main` | Stable branch — contains Task 1 + Task 3 |
| `joey-task1` | Task 1 work |
| `joey-task2` | Task 2 work |
| `joey-task3` | Task 3 work |
| `joey-task4` | Task 4 work |

## Repository Structure

```
submission/
├── task-1-decompilation/       # APK decompilation, evidence, screenshots
│   ├── apktool/
│   ├── jadx/
│   ├── evidence/
│   ├── notes/
│   ├── screenshots/
│   ├── task1-writeup-draft.md
│   └── PROGRESS_LOG.md
├── task-2-system-model/        # System model, diagram, data flow
│   ├── evidence/
│   ├── notes/
│   ├── system-model-diagram-template.md
│   ├── task2-initial-model.md
│   ├── task2-short-report-paragraph.md
│   └── PROGRESS_LOG.md
├── task-3-vulnerability-discovery/  # Candidate findings, primary vulnerability
│   ├── evidence/
│   ├── notes/
│   ├── screenshots/
│   ├── task3-candidate-findings.md
│   ├── task3-expanded-findings.md
│   ├── task3-primary-vulnerability-draft.md
│   └── PROGRESS_LOG.md
├── task-4-threat-model/         # Threat model, attack path, impact
│   ├── evidence/
│   ├── notes/
│   ├── screenshots/
│   ├── task4-threat-model-draft.md
│   ├── task4-attack-path-short.md
│   └── PROGRESS_LOG.md
└── task-5-report-and-presentation/  # Pending — final report + presentation
```

## Key Files

| File | Description |
|------|-------------|
| `a1_case1.apk` | Target APK for analysis |
| `assignment1-spec.pdf` | Assignment specification |
| `assignment1-rubric-2.pdf` | Grading rubric |
| `README_1.md` | Assignment 1 full guide |

## Submission Requirements (Assignment 1)

- `report.pdf` — max 2 pages, USENIX template
- `ai-log/`
- `pocs/`
- `presentation.mp4`
- `activity-log.pdf` — N×N contribution matrix

**Penalty reminders:**
- Missing required item: −5 marks
- Presentation > 5 min + 10 sec: −1 mark / 10 sec
- Report > 2 pages or wrong template: −3 marks
- Each member must speak ≥ 40 seconds

## Deadline

**Mon Mar 30, 2026, 5:00pm**

## Pre-submission Checklist

- [ ] Task 1–4 complete with evidence
- [ ] Report uses USENIX template, ≤ 2 pages
- [ ] Presentation ≤ 5 min
- [ ] All required files prepared
- [ ] AI usage logged in `ai-log/`
- [ ] `activity-log.pdf` uses N×N contribution matrix
- [ ] Every team member speaks ≥ 40 seconds in presentation
