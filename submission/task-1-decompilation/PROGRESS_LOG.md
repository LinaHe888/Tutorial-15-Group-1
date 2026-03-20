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
- **Files changed:** `submission/task-1-decompilation/my-task1-analysis/apktool/`, `submission/task-1-decompilation/my-task1-analysis/jadx/`, `submission/task-1-decompilation/my-task1-analysis/notes/task1-notes.md`
- **What I found:** Identified package name `com.example.mastg_test0016`, launcher activity `com.example.mastg_test0016.MainActivity`, manifest file under the apktool output, and login-related class `Login.java` in the jadx output.
- **Evidence / screenshots:** Need manifest screenshot and Login.java screenshot next.
- **Next step:** Inspect `AndroidManifest.xml` and `Login.java` in detail, then capture screenshots and record exact evidence lines.
