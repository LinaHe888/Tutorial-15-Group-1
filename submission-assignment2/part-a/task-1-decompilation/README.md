# Task 1 - Unpack and Decompile the Network APK

## Goal
Use AI and the Assignment 1 workflow to decompile `a2_case1.apk` and confirm access to the manifest, main activity, and at least one network-relevant class.

## Tools Used
- JADX
- APKTool
- AI-assisted code review

## What Was Confirmed
- Package name: `com.example.mastg_test0019`
- Manifest successfully extracted and reviewed
- Main activity identified: `com.example.mastg_test0019.MainActivity`
- Network-relevant logic identified in `MainActivity.java`

## Key Outcome
The team successfully accessed all artefacts needed for later tasks:
- the manifest
- the main activity
- network-relevant code in `MainActivity.java`

## Brief Decompilation Steps
1. Decompiled the APK with APKTool to inspect the manifest and application structure.
2. Decompiled the APK with JADX to recover readable Java code.
3. Located the app package, main activity, and network-related logic in `MainActivity.java`.

## Outcome
Task 1 is complete: the APK was successfully unpacked and decompiled, and the main network-relevant code path was identified.
