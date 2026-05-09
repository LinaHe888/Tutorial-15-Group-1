# AI Usage Log – Assignment 2 Part A Task 4

## Tools Used
- ChatGPT (GPT-5.3)
- OpenClaw Agent (Goose)

## Usage Summary

AI tools were used to assist with the following aspects of Task 4:

1. **Mitigation Research**: AI helped identify Android security best practices for addressing the insecure transport issues found in Task 3, including:
   - Google Android Developer Documentation on `usesCleartextTraffic`
   - SSL error handling guidelines (`sslErrorHandler.cancel()` vs `proceed()`)
   - Network Security Configuration schema for centralized policy enforcement
   - Certificate pinning as defense-in-depth

2. **Code-Level Fixes**: AI provided concrete code snippets showing how to replace insecure implementations with secure alternatives, matching the exact file locations and line references from Task 3 analysis.

3. **Rationale Documentation**: AI helped explain the security benefits of each proposed mitigation in the context of the MITM threat identified in Task 2/3.

All mitigation proposals were cross-referenced against the actual insecure code paths found in `MainActivity.java` and `AndroidManifest.xml` during manual static analysis. AI assisted with research, formatting, and best-practice alignment; the specific fixes target only the vulnerabilities actually present in the app.

## Human Review
- Joey verified that each mitigation directly addresses the corresponding vulnerability from Task 3.
- Confirmations were made that mitigation explanations align with the assignment rubric (concrete, justified, app-specific).
