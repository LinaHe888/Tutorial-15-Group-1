# Task 4 Evidence — Threat Model, Attack Path, and Impact

## 1. Main weakness being modelled
- **Core weakness:** the app generates its session token using `java.util.Random`.
- **Primary code location:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Login.java`
- **Why it matters:** the generated value is not a cosmetic random number; it is directly stored as the authenticated session token.

---

## 2. Evidence that the token is security-sensitive
### 2.1 Session token definition
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Login.java`
- **Lines:** 26-29
- **Evidence:**
  ```java
  private static final String KEY_SESSION_TOKEN = "sessionToken";
  private static final String SESSION_PREF_NAME = "SessionPrefs";
  private SharedPreferences.Editor editor;
  private SharedPreferences sharedPreferences;
  ```
- **Threat-model conclusion:** The app explicitly defines a session token and a dedicated preference area for storing it. This shows that the affected value is part of authentication state.

### 2.2 Session creation after successful login
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Login.java`
- **Lines:** 49-60
- **Evidence:**
  ```java
  ((Button) findViewById(R.id.button2)).setOnClickListener(new View.OnClickListener() {
      @Override
      public void onClick(View view) throws Throwable {
          boolean zCheckCredentials = Login.this.checkCredentials(editText.getText().toString(), editText2.getText().toString());
          if (!zCheckCredentials) {
              Toast.makeText(Login.this, "Wrong Credential", 0).show();
              return;
          }
          Login.this.createSession();
          Login.this.startActivity(new Intent(Login.this, (Class<?>) Profile.class));
      }
  });
  ```
- **Threat-model conclusion:** The token is created only after authentication succeeds, which confirms that it represents post-login session state.

### 2.3 Session persistence in local storage
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Login.java`
- **Lines:** 174-177
- **Evidence:**
  ```java
  public void createSession() {
      this.editor.putString(KEY_SESSION_TOKEN, generateSessionToken());
      this.editor.apply();
  }
  ```
- **Threat-model conclusion:** The app saves the generated value as `sessionToken`, so any weakness in token generation directly affects authenticated session integrity.

---

## 3. Evidence of weak randomness
### 3.1 Token generation logic
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Login.java`
- **Lines:** 183-189
- **Evidence:**
  ```java
  private String generateSessionToken() {
      Random random = new Random();
      StringBuilder sb = new StringBuilder(16);
      for (int i = 0; i < 16; i++) {
          sb.append("ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789".charAt(random.nextInt(62)));
      }
      return sb.toString();
  }
  ```
- **Threat-model conclusion:** `java.util.Random` is not a cryptographically secure source of randomness. Using it for session tokens weakens unpredictability and creates a realistic security concern.

---

## 4. Attacker model
A realistic attacker in this scenario is a **local attacker / reverse engineer** who can inspect the APK and reason about the app’s authentication logic. This attacker may be:
- a user with physical/device access,
- a reverse engineer analyzing the APK offline,
- a tester or analyst with access to local app state in a compromised or debug environment.

### Why this attacker model fits the app
- No clear backend service or server validation was identified in the reviewed code.
- The app’s authentication logic is implemented locally.
- Session state is stored locally in `SharedPreferences`.
- Credential checking also happens locally using `credentials.txt`.

Therefore, the most realistic threat model is not a remote network attacker, but someone who can inspect or reason about the client-side authentication design.

---

## 5. Attacker capabilities
Within this threat model, the attacker is assumed to be able to:
1. decompile the APK and inspect source-level logic;
2. identify the meaning of `sessionToken` and `SessionPrefs`;
3. observe that token generation uses `java.util.Random`;
4. understand the order of operations: credential check → session creation → profile access;
5. reason about or inspect local storage structures.

The attacker is **not necessarily assumed** to control any backend, because the current evidence does not show meaningful server-side verification.

---

## 6. Attack path
A simple step-by-step attack scenario is:

1. **Reverse engineer the app**
   - The attacker decompiles the APK and inspects `Login.java`.

2. **Identify authentication state handling**
   - The attacker finds that the app stores a value named `sessionToken` in `SessionPrefs`.

3. **Identify weak token generation**
   - The attacker finds that `generateSessionToken()` uses `java.util.Random`.

4. **Connect the token to authenticated state**
   - The attacker sees that `createSession()` is called only after successful credential verification and before transition to `Profile`.

5. **Exploit reduced unpredictability**
   - Because the token generator is not cryptographically secure, the attacker has a stronger basis for prediction, reproduction, or efficient guessing than would be the case with a secure random generator such as `SecureRandom`.

6. **Abuse session state confidence**
   - Even if the app is only a local demo, the design weakens trust in session authenticity because the token does not meet expected security standards for authentication secrets.

---

## 7. Impact analysis
### Direct impact
- Weakens session integrity.
- Reduces confidence that authenticated state is unpredictable.
- Makes the session mechanism weaker than expected for any security-sensitive application.

### Broader impact
- The issue affects authentication state, not just a non-critical random value.
- A weak session token undermines the reliability of post-login access control.
- The design demonstrates poor randomness hygiene in a security-sensitive context.

### Why this matters even in a simple app
Although the app appears to be a local teaching/demo app, the assignment is explicitly focused on identifying misuse of randomness or cryptographic mechanisms. In that context, the weakness is still valid and important because it shows an incorrect design pattern for session-token generation.

---

## 8. Supporting context that increases severity
### 8.1 Plaintext credential storage
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/MainActivity.java`
- **Lines:** 59-61
- **Evidence:**
  ```java
  fileOutputStreamOpenFileOutput = openFileOutput("credentials.txt", 32768);
  fileOutputStreamOpenFileOutput.write(("Username: " + str + " Password: " + str2 + "\n").getBytes());
  Toast.makeText(this, "Credentials saved to file", 0).show();
  ```
- **Why it matters:** The app already uses a weak local authentication design. This does not replace the main token issue, but it makes the overall security posture worse.

### 8.2 Local credential verification
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Login.java`
- **Lines:** 77-93
- **Evidence summary:** The app opens `credentials.txt`, reads the saved values, and compares them directly with user input.
- **Why it matters:** Authentication relies entirely on local data and local session state, making weak token generation more relevant in practice.

### 8.3 Local session clearing only
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Profile.java`
- **Lines:** 49-51
- **Evidence:**
  ```java
  public void clearSession() {
      this.editor.remove(KEY_SESSION_TOKEN);
      this.editor.apply();
  }
  ```
- **Why it matters:** Session lifecycle is handled entirely locally, reinforcing the local-attacker threat model.

---

## 9. Risk statement
This issue should be treated as a **medium-to-high risk design weakness within the scope of the assignment**, because:
- it affects a security-sensitive authentication value,
- it involves incorrect randomness usage,
- it directly influences session state,
- and it is supported by a generally weak local authentication design.

---

## 10. Recommended report positioning
In the final report, this section should be presented as:
- **Main weakness:** predictable session token generation using `java.util.Random`
- **Attacker:** local reverse engineer / local attacker with app inspection capability
- **Impact:** reduced session integrity and weaker authenticated-state assurance
- **Supporting context:** plaintext credential storage and local-only authentication/session design
