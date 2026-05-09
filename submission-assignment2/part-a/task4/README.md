# Task 4: Mitigation Ideas

## Contents

| File | Description |
|------|-------------|
| `Task4_Mitigation.md` | Concrete, code-level mitigations with Android documentation references |
| `ai-log.md` | AI tool usage disclosure for this task |

## Summary

Task 4 proposes four concrete mitigations that directly address the insecure transport vulnerabilities identified in Task 3:

1. **Enforce HTTPS Only** — disable `usesCleartextTraffic`, switch URL to `https://`
2. **Proper TLS Validation** — replace `sslErrorHandler.proceed()` with `cancel()`
3. **Secure Hostname Verification** — remove custom `HostnameVerifier`, rely on platform defaults
4. **Network Security Configuration** — add XML policy for centralized cleartext restriction and trust anchor control

Each mitigation is justified against the specific code paths found in the app and aligned with Google Android security best practices.
