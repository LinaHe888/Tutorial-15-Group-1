# Assignment 2 Part A

## Purpose
This directory contains the Part A materials for Assignment 2. Part A focuses on network-security analysis of the provided Android APK.

## Rubric Scope Reminder
Part A should focus only on MITM-relevant transport/TLS weaknesses, especially:
- cleartext traffic
- certificate validation failures
- hostname validation failures

## Current Structure
- `task-1-decompilation/README.md`: decompilation steps and tooling
- `task-2-network-model/README.md`: app summary and network-focused model
- `task-3-vulnerability/README.md`: primary vulnerability and impact
- `task-4-mitigation/README.md`: mitigation ideas
- `evidence/code-evidence.md`: code snippets and brief interpretation
- `evidence/evidence-summary.md`: short evidence overview
- `report/report.md`: current report draft
- `video/parta-video-script.md`: current video script
- `notes/`: working notes and validation plan

## Current Primary Direction
The current primary finding is **cleartext traffic enabled and used**, supported by weak SSL handling in `WebView`.

## Remaining Work
- capture runtime evidence
- export final `report.pdf`
- record final `parta-presentation.mp4`
