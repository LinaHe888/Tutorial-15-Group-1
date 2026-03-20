# Task 1 Progress Log

> 分支：`joey-task1`
> 要求：每完成一个可描述的进度，就记录 + commit + push。

## Log Template

### [YYYY-MM-DD HH:MM]
- **Step:**
- **What I did:**
- **Commands used:**
- **Files changed:**
- **What I found:**
- **Evidence / screenshots:**
- **Next step:**

---

## Progress Entries

### [2026-03-21 02:24]
- **Step:** Initial setup
- **What I did:** Created Task 1 working branch and prepared analysis workspace.
- **Commands used:** `git checkout -b joey-task1`, tool setup, created folders and notes template.
- **Files changed:** `submission/task-1-decompilation/my-task1-analysis/`, `submission/task-1-decompilation/PROGRESS_LOG.md`
- **What I found:** `apktool` and `jadx` are installed and ready.
- **Evidence / screenshots:** N/A
- **Next step:** Run `apktool` and `jadx` on `a1_case1.apk` and start extracting manifest/app entry information.


### [2026-03-21 02:31]
- **Step:** Decompile APK and verify basic app entry points
- **What I did:** Set `JAVA_HOME` to JDK 22, ran `apktool` to decode the APK, and ran `jadx` to generate readable Java source code.
- **Commands used:** `export JAVA_HOME=$(/usr/libexec/java_home -v 22)`, `export PATH="$JAVA_HOME/bin:$PATH"`, `apktool d a1_case1.apk -o submission/task-1-decompilation/my-task1-analysis/apktool -f`, `jadx -d submission/task-1-decompilation/my-task1-analysis/jadx a1_case1.apk`
- **Files changed:** `submission/task-1-decompilation/apktool/`, `submission/task-1-decompilation/jadx/`, `submission/task-1-decompilation/notes/task1-notes.md`
- **What I found:** Identified package name `com.example.mastg_test0016`, launcher activity `com.example.mastg_test0016.MainActivity`, manifest file under the apktool output, and login-related class `Login.java` in the jadx output.
- **Evidence / screenshots:** Need manifest screenshot and Login.java screenshot next.
- **Next step:** Inspect `AndroidManifest.xml` and `Login.java` in detail, then capture screenshots and record exact evidence lines.


### [2026-03-21 02:35]
- **Step:** Organize Task 1 evidence from manifest and Login.java
- **What I did:** Treated Joey's uploaded screenshot as evidence, copied it into the Task 1 evidence folder, and extracted exact evidence from `AndroidManifest.xml` and `Login.java`.
- **Commands used:** `cp ... step-1-terminal-screenshot.jpg`, `nl -ba ... AndroidManifest.xml`, `nl -ba ... Login.java`
- **Files changed:** `submission/task-1-decompilation/evidence/step-1-terminal-screenshot.jpg`, `submission/task-1-decompilation/evidence/manifest-and-login-evidence.md`, `submission/task-1-decompilation/notes/task1-notes.md`
- **What I found:** Confirmed package name, launcher activity, and login-related class with exact file paths and evidence lines. Also identified session token creation logic for later tasks.
- **Evidence / screenshots:** Terminal screenshot uploaded; manifest and Login.java evidence organized into a dedicated markdown file.
- **Next step:** Capture code/editor screenshots for the manifest and `Login.java`, or move on to drafting the Task 1 write-up using the recorded evidence.


### [2026-03-21 02:39]
- **Step:** Draft Task 1 write-up in English
- **What I did:** Converted the collected manifest and Login.java evidence into a structured English draft for Task 1.
- **Commands used:** Created `submission/task-1-decompilation/task1-writeup-draft.md`
- **Files changed:** `submission/task-1-decompilation/task1-writeup-draft.md`
- **What I found:** The current evidence is already sufficient to support a solid Task 1 write-up covering APK definition, decompilation purpose, tools used, and key findings.
- **Evidence / screenshots:** Based on previously recorded manifest, Login.java, and uploaded screenshot evidence.
- **Next step:** Refine wording for the final report format or continue extracting more screenshots if needed.


### [2026-03-21 02:43]
- **Step:** Prepare screenshot plan and captions for Task 1
- **What I did:** Created a screenshot checklist for the final Task 1 write-up and added ready-to-use English figure captions.
- **Commands used:** Created `submission/task-1-decompilation/screenshot-plan-and-captions.md`
- **Files changed:** `submission/task-1-decompilation/screenshot-plan-and-captions.md`
- **What I found:** Three screenshots are already enough for a strong Task 1 evidence set: terminal/setup, manifest, and Login.java.
- **Evidence / screenshots:** Existing terminal screenshot plus planned manifest and Login.java screenshots.
- **Next step:** Capture the manifest and Login.java screenshots, or refine the Task 1 draft into report-ready wording.


### [2026-03-21 03:09]
- **Step:** Add manifest and Login.java screenshots as Task 1 evidence
- **What I did:** Added the selected manifest screenshot and Login.java screenshot to the evidence folder for Task 1.
- **Commands used:** copied inbound screenshots into `submission/task-1-decompilation/evidence/`
- **Files changed:** `submission/task-1-decompilation/evidence/manifest-screenshot.jpg`, `submission/task-1-decompilation/evidence/login-java-screenshot.jpg`
- **What I found:** Both screenshots are clear enough to support package/main activity evidence and login flow evidence.
- **Evidence / screenshots:** manifest screenshot and Login.java screenshot saved in evidence folder.
- **Next step:** Either refine Task 1 write-up into final report wording or move on to Task 2 system model work.
