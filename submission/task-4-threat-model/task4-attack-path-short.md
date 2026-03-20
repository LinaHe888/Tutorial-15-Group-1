# Task 4 Short Attack Path

An attacker can reverse engineer the APK and identify that the app stores an authenticated session value under `sessionToken` in `SessionPrefs`. The attacker can then inspect `generateSessionToken()` and see that the token is produced using `java.util.Random` rather than a cryptographically secure generator. Because session tokens should be unpredictable, this weak design reduces confidence in the security of the authenticated session state. The impact is further increased by the app's local plaintext credential storage and simple local session handling.
