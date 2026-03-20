# Task 1 Notes

## Tools used
- apktool 3.0.1
- jadx 1.5.5

## APK
- File: a1_case1.apk

## Key findings
- Package name: `com.example.mastg_test0016`
- Main activity: `com.example.mastg_test0016.MainActivity`
- Manifest path: `submission/task-1-decompilation/my-task1-analysis/apktool/AndroidManifest.xml`
- Login/auth related class: `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Login.java`

## Evidence
- Screenshot 1: Uploaded terminal screenshot `evidence/step-1-terminal-screenshot.jpg`
- Screenshot 2: Manifest package name + launcher activity
- Screenshot 3: Login.java class and login flow
- Evidence summary doc: `evidence/manifest-and-login-evidence.md`

## Commands run
- `export JAVA_HOME=$(/usr/libexec/java_home -v 22)`
- `export PATH="$JAVA_HOME/bin:$PATH"`
- `apktool d a1_case1.apk -o submission/task-1-decompilation/my-task1-analysis/apktool -f`
- `jadx -d submission/task-1-decompilation/my-task1-analysis/jadx a1_case1.apk`

## Notes
- `jadx` completed with decompilation errors reported at the end (`count: 19`), but source output was still generated and can be inspected.
