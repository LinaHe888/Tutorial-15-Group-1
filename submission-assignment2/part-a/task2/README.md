# Task 2: Understand the App and Build a Network-Focused Model

## Contents

| File | Description |
|------|-------------|
| `Task2_Network_Model.md` | Application overview, system architecture, and STRIDE threat model |
| `ai-log.md` | AI tool usage disclosure for this task |

## Summary

Task 2 identifies the app as a minimal WebView-based browser (`com.example.mastg_test0019`) that loads `http://www.example.com` over cleartext HTTP with all TLS validation disabled.

The deliverable includes:
1. **App understanding** — single-activity WebView app with no local storage or authentication
2. **Network behavior** — HTTP GET over port 80, DNS resolution, response rendering
3. **System model** — ASCII architecture diagram showing device → OS → network → server flow
4. **Threat model** — STRIDE-aligned analysis with three concrete MITM attack scenarios (eavesdropping, certificate impersonation, hostname spoofing)
