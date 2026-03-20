# Task 1 Evidence — Manifest and Login.java

## 1. Package name evidence
- **File:** `submission/task-1-decompilation/my-task1-analysis/apktool/AndroidManifest.xml`
- **Line:** 2
- **Evidence:**
  ```xml
  <manifest ... package="com.example.mastg_test0016" ...>
  ```
- **Conclusion:** The app package name is `com.example.mastg_test0016`.

## 2. Main activity / launcher evidence
- **File:** `submission/task-1-decompilation/my-task1-analysis/apktool/AndroidManifest.xml`
- **Lines:** 8-12
- **Evidence:**
  ```xml
  <activity android:exported="true" android:name="com.example.mastg_test0016.MainActivity">
      <intent-filter>
          <action android:name="android.intent.action.MAIN"/>
          <category android:name="android.intent.category.LAUNCHER"/>
      </intent-filter>
  </activity>
  ```
- **Conclusion:** `com.example.mastg_test0016.MainActivity` is the launcher activity.

## 3. Login-related class evidence
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Login.java`
- **Lines:** 25-29
- **Evidence:**
  ```java
  public class Login extends AppCompatActivity {
      private static final String KEY_SESSION_TOKEN = "sessionToken";
      private static final String SESSION_PREF_NAME = "SessionPrefs";
  }
  ```
- **Conclusion:** `Login.java` is an authentication-related activity and manages session token storage.

## 4. Login flow evidence
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
- **Conclusion:** The login activity verifies credentials and, on success, creates a session before opening the profile screen.

## 5. Credential checking evidence
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Login.java`
- **Lines:** 77-93
- **Evidence summary:** The app opens `credentials.txt`, reads stored values, and compares them with the user input.
- **Conclusion:** The login logic is implemented in `Login.java`, making it valid Task 1 evidence for an authentication-related class.

## 6. Session token generation evidence (useful for later tasks)
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Login.java`
- **Lines:** 174-189
- **Evidence summary:** `createSession()` stores `generateSessionToken()`, which uses `java.util.Random`.
- **Conclusion:** This is strong candidate evidence for later vulnerability analysis in Task 3.

## Uploaded screenshot evidence
- `submission/task-1-decompilation/my-task1-analysis/evidence/step-1-terminal-screenshot.jpg`
