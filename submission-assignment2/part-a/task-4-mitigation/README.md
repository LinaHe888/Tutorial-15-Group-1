# Task 4 - Mitigation Ideas

## Goal
Propose concrete mitigations and explain why they reduce or remove risk for this app.

## Recommended Mitigations
1. Disable cleartext traffic and require HTTPS for all remote content.
2. Do not ignore SSL certificate errors in `WebView`.
3. Avoid permissive hostname verification and rely on platform-default HTTPS checks.

## Why These Fixes Work
- Disabling cleartext traffic prevents attackers from reading or modifying HTTP content in transit.
- Enforcing proper certificate validation prevents the app from trusting attacker-controlled or invalid certificates.
- Enforcing proper hostname verification ensures that the app connects only to the intended server identity.

Together, these changes restore confidentiality, integrity, and authenticity for the app’s network traffic.

## Outcome
Task 4 is complete at the design level. The mitigation plan directly addresses the transport security weaknesses identified in Task 3.
