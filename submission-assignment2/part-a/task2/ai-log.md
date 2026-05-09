# AI Usage Log – Assignment 2 Part A Task 2

## Tools Used
- ChatGPT (GPT-5.3)
- OpenClaw Agent (Goose) with exec tool

## Usage Summary

AI tools were used to assist with the following aspects of Task 2:

1. **Code Analysis**: The AI read the decompiled source code (`MainActivity.java` and `AndroidManifest.xml`) from Task 1 output to understand app functionality and network behavior.

2. **System Model Construction**: The AI generated ASCII architecture diagrams and component tables based on static analysis findings, showing the data flow from app → OS network stack → internet → server.

3. **Threat Model Development**: The AI created STRIDE-aligned threat diagrams and attack scenarios (cleartext eavesdropping, certificate impersonation, hostname spoofing) based on the insecure transport patterns identified in the code.

4. **Documentation Formatting**: AI organized findings into structured markdown with tables, diagrams, and clear sectioning suitable for academic submission.

All technical content — app behavior, network flows, vulnerability patterns, and threat classifications — was derived from direct manual inspection of the decompiled application code. AI assisted with presentation, diagram generation, and documentation structure only.

## Human Review
- Joey reviewed the generated threat model for accuracy against the assignment rubric.
- Adjustments were made to ensure the model explicitly covers the three required elements: app summary, data over network, and on-path attacker scenario.
