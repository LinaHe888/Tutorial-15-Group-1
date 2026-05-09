# Task 1: Unpack and Decompile the Network APK

## Contents

| File | Description |
|------|-------------|
| `a2_partA_task1.zip` | Decompilation output (jadx + apktool) containing manifest, smali, and Java sources |

## Summary

Task 1 involved decompiling `a2_case1.apk` using `jadx` and `apktool` to obtain:
- `AndroidManifest.xml` — app permissions and cleartext traffic flag
- `MainActivity.java` — WebView usage, URL loading, SSL error handling
- Resources and smali bytecode for full static analysis

All findings from Task 1 feed into Tasks 2–4.
