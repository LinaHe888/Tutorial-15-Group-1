# Assignment 2 Part A

## Purpose
This directory contains the Part A materials for Assignment 2. Part A focuses on network-security analysis of the provided Android APK.

## Rubric Scope Reminder
Part A should focus only on MITM-relevant transport security issues, especially:
- cleartext traffic
- certificate validation failures
- hostname validation failures

## Current Contents
- `report/`: reserved for the final Part A report (`report.pdf`)
- `video/`: reserved for the final Part A video (`parta-presentation.mp4`)
- `evidence/code-evidence.md`: current static code evidence
- `notes/initial-static-scan.md`: first-round Part A static findings
- `notes/system-and-threat-model-draft.md`: draft system and threat model
- `notes/dynamic-validation-plan.md`: runtime validation plan
- `README.md`: this file

## Current Direction
The current strongest Part A candidates are:
1. cleartext traffic enabled and used (`usesCleartextTraffic="true"` + `webView.loadUrl("http://...")`)
2. broken WebView SSL handling (`onReceivedSslError(...){ proceed(); }`)

## Next Step
Validate the strongest candidate dynamically and then convert the result into:
- the final Part A report
- the final Part A video
- supporting evidence files
