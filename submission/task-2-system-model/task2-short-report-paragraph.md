# Task 2 — Short Report Paragraph + Video Script

## Report Paragraph

The app is a simple local authentication demo with three components. `MainActivity` acts as the launcher, allowing users to register by entering a username and password, which are written to a local file `credentials.txt`. The `Login` screen reads this file, compares the supplied credentials, and on success generates a session token stored in `SharedPreferences` (`SessionPrefs`). The `Profile` screen represents the authenticated state, reading the session token from `SessionPrefs` to determine access. The key security-relevant assets are the credentials file, the session token, and the shared preferences storage.

---

## Video Script (Task 2 — ~45 seconds)

**[0-10s] App Overview**
"This app is a local authentication demo with three screens. `MainActivity` handles registration, storing username and password in a local file. `Login` verifies credentials and creates a session. `Profile` is the protected area."

**[10-20s] Registration Flow**
"When a user registers, MainActivity writes the credentials to `credentials.txt` in plain text, then navigates to the login screen. This is the only credential storage mechanism in the app."

**[20-32s] Login and Session Creation**
"When the user logs in, Login reads `credentials.txt`, compares the input, and if it matches, calls `createSession()`. This generates a session token and stores it in `SharedPreferences`. The app then opens the Profile screen."

**[32-45s] Trust Boundary and Security Relevance**
"The key trust boundary is between the unauthenticated state — where an attacker only needs to know the credentials file — and the authenticated state, where the session token in `SharedPreferences` represents proof of login. This is the critical link for Task 3's vulnerability."
