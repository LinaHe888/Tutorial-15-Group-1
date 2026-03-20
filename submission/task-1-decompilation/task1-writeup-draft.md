# Task 1 Draft — Unpack and Decompile the APK

## What is an APK?
An APK (Android Package Kit) is the standard package format used to distribute and install Android applications. It typically contains the compiled application code, Android manifest, resources, assets, and metadata required for execution on an Android device.

## Why decompile the APK?
Because the original source code is not directly provided, decompilation is necessary to inspect the app structure and implementation details. Reverse engineering allows us to identify the package name, launcher activity, manifest configuration, and authentication-related classes, which provides the technical foundation for later system modelling and vulnerability discovery.

## Tools used
For this task, we used the following tools:

- **apktool** — to decode the APK, inspect the manifest, resources, and lower-level app structure.
- **jadx** — to decompile the APK into readable Java source code for easier code inspection.

In our environment, `jadx` initially failed because the shell was using an incompatible Java runtime. We corrected this by setting `JAVA_HOME` to JDK 22 before re-running the tool.

## Key findings from decompilation
After decompiling the provided APK (`a1_case1.apk`), we identified the following key artifacts:

- **Package name:** `com.example.mastg_test0016`
- **Main activity:** `com.example.mastg_test0016.MainActivity`
- **Manifest file:** `submission/task-1-decompilation/apktool/AndroidManifest.xml`
- **Authentication-related class:** `submission/task-1-decompilation/jadx/sources/com/example/mastg_test0016/Login.java`

## Evidence summary
The package name was identified in the manifest declaration. The launcher activity was confirmed through the `MAIN` and `LAUNCHER` intent filter in `AndroidManifest.xml`. We also located `Login.java`, which contains the login flow, credential checking logic, and session creation logic. These findings confirm that the APK was successfully unpacked and that the main app entry points were recovered.

## Why this matters for later tasks
This decompilation step provides the basis for the rest of the assignment. The manifest helps define the app structure and entry points, while the recovered Java code allows us to inspect authentication logic and identify security-relevant behavior in later tasks.

## Notes on decompilation quality
`jadx` completed with a small number of decompilation errors, but the required source output was still generated successfully. The main app classes relevant to this task, including `MainActivity`, `Login`, and `Profile`, were available for inspection.
