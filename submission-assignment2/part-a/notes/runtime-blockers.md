# Runtime Validation Blockers

## Current Environment Limitation
During this session, static analysis was completed successfully using JADX and APKTool. However, runtime validation has not yet been completed from this environment because Android runtime tooling such as `adb` is not currently available in the shell path.

## What Still Needs to Be Captured
To fully finish Part A with runtime support, collect:
- a screenshot or capture proving HTTP traffic is used at runtime, or
- a screenshot proving `WebView` continues after an SSL error under a controlled MITM test

## Recommended Runtime Path
1. Run `a2_case1.apk` in emulator or device.
2. Route traffic through Burp or another controlled proxy.
3. Confirm cleartext HTTP request to `http://www.example.com`, or confirm SSL bypass behaviour in `WebView`.
4. Save screenshots / captures into `submission-assignment2/part-a/evidence/`.
