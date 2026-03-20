# Task 4 Screenshot Index

Task 4 can reuse Task 3 screenshots because the threat model is derived from the same code path.

## Reused screenshots
- `../task-3-vulnerability-discovery/evidence/create-session-screenshot.jpg`
  - supports session-token storage evidence
- `../task-3-vulnerability-discovery/evidence/generate-session-token-screenshot.jpg`
  - supports weak randomness evidence
- `../task-3-vulnerability-discovery/evidence/login-flow-screenshot.jpg`
  - supports post-authentication session creation evidence
- `../task-3-vulnerability-discovery/evidence/plaintext-credentials-screenshot.jpg`
  - supports broader insecure local-authentication context

## Suggested figure captions
- **Figure 1.** The app stores a generated value as `sessionToken` in `SessionPrefs` after successful login.
- **Figure 2.** The session token is generated using `java.util.Random`, which is not a cryptographically secure random source.
- **Figure 3.** The login flow shows that session creation occurs only after credential verification.
- **Figure 4.** Credentials are stored in plaintext in `credentials.txt`, strengthening the local-attacker threat model.
