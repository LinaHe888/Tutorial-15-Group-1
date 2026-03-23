# Task 2 Initial System Model

## 1. App summary

This app appears to be a small local authentication demo. The user can register by entering a username and password on the main screen, and the app stores those credentials in a local file. The user can then navigate to the login screen, submit the same credentials, and if the check succeeds, the app creates a session token and opens a profile screen. The profile screen also supports logging out by clearing the stored session token.

## 2. Main components

### 2.1 MainActivity

- Acts as the launcher activity.
- Provides the initial interface for user input.
- Supports registration by collecting username and password.
- Writes the credentials into `credentials.txt` using local app storage.
- Can also navigate directly to the login screen.

### 2.2 Login

- Provides the login interface.
- Reads the locally stored `credentials.txt` file.
- Compares the entered username and password with stored values.
- If authentication succeeds, creates a session token and saves it in shared preferences.
- Navigates to the profile screen after successful login.

### 2.3 Profile

- Represents the post-login screen.
- Reads and uses the same session preference area.
- Allows the user to log out by clearing the stored session token.

### 2.4 Local storage

The app uses two local storage mechanisms:

- `credentials.txt` for storing username and password.
- `SharedPreferences` (`SessionPrefs`) for storing the session token.

## 3. Key assets

The main security-relevant assets in this app are:

- **Username** — user identity input.
- **Password** — authentication secret.
- **Stored credentials file (`credentials.txt`)** — local persistent credential storage.
- **Session token** — used to represent an authenticated session.
- **Session preference storage (`SessionPrefs`)** — location where the session token is stored.

## 4. Data flow

### Registration flow

1. The user enters a username and password in `MainActivity`.
2. The app writes the values into `credentials.txt`.
3. The app redirects the user to `Login`.

### Login flow

1. The user enters a username and password in `Login`.
2. The app opens `credentials.txt` and reads the stored values.
3. The app compares the input with the stored credentials.
4. If the values match, the app creates a session token.
5. The app stores the session token in `SharedPreferences`.
6. The app opens `Profile`.

### Logout flow

1. The user presses the logout button in `Profile`.
2. The app removes the stored session token from shared preferences.

## 5. Trust boundaries and assumptions

- The app appears to operate mainly on the local device.
- No clear remote backend or network API was identified in the decompiled code reviewed so far.
- The main trust boundary is between user-controlled input and app-controlled local storage.
- Another important boundary exists between unauthenticated state (`MainActivity` / `Login`) and authenticated state (`Profile`).
- For now, the analysis assumes the app relies on local files and shared preferences only.

## 6. Security analysis boundary

This Task 2 model focuses on:

- app screens and their roles,
- local storage of credentials and session state,
- data movement between activities,
- security-sensitive assets relevant to authentication and later vulnerability analysis.

This model does not yet assume any backend server because no strong server-side interaction evidence has been identified in the reviewed files.

## 7. Suggested simple system model diagram

A simple diagram for the report can be drawn as:

User
-> MainActivity (register / navigate)
-> credentials.txt (local storage)
-> Login (credential check)
-> SharedPreferences / SessionPrefs (session token)
-> Profile (authenticated view)

You can also represent this with trust boundaries around:

- UI / Activities
- Local file storage
- SharedPreferences session storage

