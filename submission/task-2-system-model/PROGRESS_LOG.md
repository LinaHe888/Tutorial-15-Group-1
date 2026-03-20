# Task 2 Progress Log


### [2026-03-21 03:10]
- **Step:** Build initial Task 2 system model
- **What I did:** Reviewed `MainActivity`, `Login`, `Profile`, and layout files to derive the app summary, components, key assets, data flows, and trust boundaries.
- **Commands used:** inspected decompiled Java files and layout XML files from Task 1 output.
- **Files changed:** `submission/task-2-system-model/task2-initial-model.md`, `submission/task-2-system-model/PROGRESS_LOG.md`
- **What I found:** The app is a local authentication demo with registration, local credential storage, login verification, session token creation, and logout.
- **Evidence / screenshots:** Based on `MainActivity.java`, `Login.java`, `Profile.java`, and `activity_main.xml`, `activity_login.xml`, `activity_profile.xml`.
- **Next step:** Turn this into a cleaner diagram and shorter report-ready summary.


### [2026-03-21 03:12]
- **Step:** Add Task 2 diagram template and short report paragraph
- **What I did:** Converted the initial system model into a simple text diagram template and a shorter report-ready paragraph.
- **Commands used:** created `system-model-diagram-template.md` and `task2-short-report-paragraph.md`
- **Files changed:** `submission/task-2-system-model/system-model-diagram-template.md`, `submission/task-2-system-model/task2-short-report-paragraph.md`
- **What I found:** The app can be clearly modelled as a local authentication flow with two storage locations: `credentials.txt` and `SessionPrefs`.
- **Evidence / screenshots:** Based on previously reviewed Java and layout files.
- **Next step:** Redraw the system model as a figure or continue to Task 3 vulnerability discovery.

