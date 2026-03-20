# Task 2 Screenshot Plan and Captions

## Recommended screenshots for Task 2

### Screenshot 1 — MainActivity / activity_main
**What to capture:**
- `MainActivity.java` registration/login flow or `activity_main.xml`
- Username field, password field, Register button, Login button

**Suggested caption:**
> Figure 1. Main activity showing the registration and login entry interface.

---

### Screenshot 2 — Login / activity_login
**What to capture:**
- `Login.java` login flow or `activity_login.xml`
- credential input + call to `checkCredentials()` + `createSession()`

**Suggested caption:**
> Figure 2. Login component showing credential verification and session creation.

---

### Screenshot 3 — Profile / activity_profile
**What to capture:**
- `Profile.java` or `activity_profile.xml`
- logout button and local session clearing behavior

**Suggested caption:**
> Figure 3. Profile screen representing the authenticated state and logout action.

---

### Screenshot 4 — Local storage evidence (optional)
**What to capture:**
- `MainActivity.saveCredentialsToFile()`
- `Login.createSession()` / `SessionPrefs`

**Suggested caption:**
> Figure 4. Local storage of credentials and session state used in the app’s authentication model.

---

## Best minimum set
If you only use 3 screenshots:
1. MainActivity
2. Login
3. Profile

## Suggested placement in write-up
1. App summary -> Figure 1
2. Data flow / authentication logic -> Figure 2
3. Authenticated state / logout -> Figure 3
4. Asset/storage discussion -> Figure 4 (optional)
