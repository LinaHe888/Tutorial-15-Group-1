# Task 3: Find and Explain the Network Vulnerability

## Contents

| File | Description |
|------|-------------|
| `Task3_Detailed.md` | Full vulnerability analysis with code evidence, attacker capabilities, and impact |
| `Task3_Report.md` | One-page executive summary of the insecure transport vulnerability |
| `Task3_AI_log.md` | AI tool usage disclosure for this task |

## Summary

Task 3 identifies the **insecure transport/TLS configuration** vulnerability class (the only class graded in Part A per the assignment spec).

Key findings:
- `android:usesCleartextTraffic="true"` in AndroidManifest.xml
- `webView.loadUrl("http://www.example.com")` — active cleartext usage
- `sslErrorHandler.proceed()` — SSL certificate validation bypassed
- Custom `HostnameVerifier` returning `true` — hostname verification disabled

The analysis demonstrates how an on-path attacker can perform MITM attacks to read, modify, or inject network traffic, breaking confidentiality, integrity, and authenticity guarantees.
