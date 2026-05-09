# AI Usage Log – Assignment 2 Part A Task 2

## Tools Used
- OpenClaw Agent (Goose) with exec and read tools
- Kimi/kimi-code model

## Usage Summary

AI tools were used in the following ways during Task 2:

1. **Decompiled Code Inspection**: The AI used `exec` to extract and read `MainActivity.java` and `AndroidManifest.xml` from the Task 1 decompilation output (`a2_partA_task1.zip`) to identify app behavior, network requests, and insecure configuration flags.

2. **System & Threat Model Drafting**: Based on the code findings, the AI generated ASCII diagrams, component tables, and STRIDE-aligned threat scenarios (cleartext eavesdropping, certificate impersonation, hostname spoofing). These were structured into the final markdown report.

3. **Assignment Rubric Alignment**: The AI ensured the deliverable explicitly addressed the three required elements: app summary, data traveling over the network, and on-path attacker modeling (same Wi-Fi / MITM).

## Human Direction
- Joey instructed the AI to follow the assignment spec requirements for Task 2.
- Joey requested the rewrite of this AI log after reviewing the initial draft.

## Scope of AI Assistance
- AI performed hands-on code reading and report generation.
- All vulnerability patterns and insecure configurations referenced in the threat model were discovered through direct inspection of the decompiled source.
