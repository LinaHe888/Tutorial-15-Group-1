# Task 4 Threat Model Draft

## 1. Main weakness
The main weakness is that the app generates its session token using `java.util.Random` in `Login.generateSessionToken()`. Because the token is used as the authenticated session identifier, this is a security-sensitive randomness problem.

## 2. Attacker model
A realistic attacker for this app is a local attacker who can inspect the app, analyze its code, and observe or interact with its authentication workflow. This attacker may be:
- a user with access to the device,
- another app or analyst with access to local app data in a testing or compromised environment,
- a reverse engineer who can inspect the APK and understand how session state is created.

## 3. Attacker capabilities
The attacker is assumed to be able to:
- reverse engineer the APK and inspect the decompiled source code,
- understand that `sessionToken` is created after login,
- observe that the token is generated using `java.util.Random`,
- inspect or reason about local session storage in `SharedPreferences`.

The attacker is **not necessarily assumed** to control a backend server, because no clear backend interaction has been identified in the reviewed code.

## 4. Attack path
A simple attack path is:
1. The attacker decompiles the APK and identifies the login/session logic.
2. The attacker finds that `generateSessionToken()` uses `java.util.Random`.
3. The attacker observes that the generated value is stored as `sessionToken` in `SessionPrefs`.
4. Because the session token is security-sensitive, weak randomness reduces trust in the unpredictability of authenticated session state.
5. In a stronger attack setting, the attacker may attempt to predict, reproduce, or brute-force valid token values more effectively than if the app used a cryptographically secure random source.

## 5. Impact
The direct impact is a weakening of session integrity. Since the token represents authenticated state, predictable token generation may make it easier to compromise or imitate a valid session. Even if the app is a simple local demonstration app, the design still violates secure randomness expectations for authentication tokens.

## 6. Supporting weakness context
The threat is strengthened by the app's broader local security design:
- credentials are stored in plaintext in `credentials.txt`,
- authentication is checked locally against that file,
- session state is stored locally in shared preferences.

These weaknesses do not replace the main randomness issue, but they increase the seriousness of the overall design.

## 7. Risk statement
This issue should be treated as a medium-to-high risk design weakness within the scope of the assignment, because it affects authentication state and directly involves randomness misuse in a security-sensitive context.
