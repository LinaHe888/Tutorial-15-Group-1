# Task 2 Evidence — System Model, Components, Assets, and Data Flow

## 1. App purpose evidence
### 1.1 Main screen purpose
- **File:** `submission/task-1-decompilation/my-task1-analysis/apktool/res/layout/activity_main.xml`
- **Lines:** 5-11
- **Evidence summary:** The main screen contains a title, overview text, username input, password input, and buttons labelled `Register` and `Login`.
- **Conclusion:** The app appears to be a simple local authentication demo focused on registration and login.

### 1.2 MainActivity logic
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/MainActivity.java`
- **Lines:** 24-49
- **Evidence:**
  ```java
  setContentView(R.layout.activity_main);
  final EditText editText = (EditText) findViewById(R.id.editTextText);
  final EditText editText2 = (EditText) findViewById(R.id.editTextTextPassword);
  Button button = (Button) findViewById(R.id.button);
  Button button2 = (Button) findViewById(R.id.lg);
  ```
- **Conclusion:** `MainActivity` is the launcher interface where the user can enter credentials and either register or navigate to login.

---

## 2. Component evidence
### 2.1 MainActivity as launcher and registration interface
- **Manifest file:** `submission/task-1-decompilation/my-task1-analysis/apktool/AndroidManifest.xml`
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
- **Conclusion:** `MainActivity` is the app entry point.

### 2.2 Login as authentication component
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Login.java`
- **Lines:** 25-29
- **Evidence:**
  ```java
  public class Login extends AppCompatActivity {
      private static final String KEY_SESSION_TOKEN = "sessionToken";
      private static final String SESSION_PREF_NAME = "SessionPrefs";
  }
  ```
- **Conclusion:** `Login` is the authentication-related activity and also manages session state.

### 2.3 Profile as authenticated state screen
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Profile.java`
- **Lines:** 21-40
- **Evidence summary:** `Profile` loads `activity_profile`, opens `SessionPrefs`, and exposes a logout button that clears session state.
- **Conclusion:** `Profile` represents the authenticated post-login screen.

---

## 3. Asset evidence
### 3.1 Credentials as key assets
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/MainActivity.java`
- **Lines:** 59-61
- **Evidence:**
  ```java
  fileOutputStreamOpenFileOutput = openFileOutput("credentials.txt", 32768);
  fileOutputStreamOpenFileOutput.write(("Username: " + str + " Password: " + str2 + "\n").getBytes());
  ```
- **Conclusion:** Username and password are security-sensitive assets because they are collected from the user and stored locally.

### 3.2 Session token as key asset
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Login.java`
- **Lines:** 174-177
- **Evidence:**
  ```java
  public void createSession() {
      this.editor.putString(KEY_SESSION_TOKEN, generateSessionToken());
      this.editor.apply();
  }
  ```
- **Conclusion:** The generated session token is a core authentication asset because it represents post-login session state.

### 3.3 Local storage as asset container
- **Locations:**
  - `credentials.txt`
  - `SessionPrefs`
- **Conclusion:** Local storage locations are part of the system model because they hold sensitive authentication data.

---

## 4. Data flow evidence
### 4.1 Registration flow
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/MainActivity.java`
- **Lines:** 31-39
- **Evidence:**
  ```java
  public void onClick(View view) {
      String string = editText.getText().toString();
      String string2 = editText2.getText().toString();
      if (!string.equals("") && !string2.equals("")) {
          MainActivity.this.saveCredentialsToFile(string, string2);
          MainActivity.this.startActivity(new Intent(MainActivity.this, (Class<?>) Login.class));
          return;
      }
  }
  ```
- **Conclusion:** Registration flow is: user input -> local file write -> transition to login.

### 4.2 Login flow
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Login.java`
- **Lines:** 49-60
- **Evidence:**
  ```java
  boolean zCheckCredentials = Login.this.checkCredentials(editText.getText().toString(), editText2.getText().toString());
  if (!zCheckCredentials) {
      Toast.makeText(Login.this, "Wrong Credential", 0).show();
      return;
  }
  Login.this.createSession();
  Login.this.startActivity(new Intent(Login.this, (Class<?>) Profile.class));
  ```
- **Conclusion:** Login flow is: user input -> credential check -> session creation -> profile access.

### 4.3 Logout flow
- **File:** `submission/task-1-decompilation/my-task1-analysis/jadx/sources/com/example/mastg_test0016/Profile.java`
- **Lines:** 35-39 and 49-51
- **Evidence summary:** The logout button calls `clearSession()`, which removes `sessionToken` from shared preferences.
- **Conclusion:** Logout flow is: user action -> local session removal.

---

## 5. Trust boundary evidence
### 5.1 Boundary between user input and app logic
- The app collects username/password from UI input fields in `MainActivity` and `Login`.
- These values cross from user-controlled input into app-controlled authentication logic.
- **Conclusion:** A trust boundary exists between user input and internal credential/session processing.

### 5.2 Boundary between app logic and local storage
- The app writes credentials into `credentials.txt` and stores session state in `SessionPrefs`.
- **Conclusion:** Another trust boundary exists between app logic and persistent local storage.

### 5.3 Boundary between unauthenticated and authenticated state
- The app transitions from `Login` to `Profile` only after credential verification and session creation.
- **Conclusion:** There is an application-state boundary between unauthenticated and authenticated views.

---

## 6. System model summary
A minimal system model for this app is:
- **User** interacts with `MainActivity` and `Login`
- **MainActivity** collects credentials and stores them in `credentials.txt`
- **Login** reads credentials, verifies them, and creates a session token
- **SharedPreferences / SessionPrefs** stores the authenticated session token
- **Profile** represents authenticated state and supports logout

This is sufficient to describe the app structure, key assets, and main data flows for Task 2.
