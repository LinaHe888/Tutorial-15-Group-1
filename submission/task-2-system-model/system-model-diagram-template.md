# Task 2 System Model Diagram Template

## Simple diagram (can be redrawn in draw.io / PowerPoint)

```text
+-------------------+
|       User        |
+-------------------+
          |
          v
+-------------------+
|   MainActivity    |
| - register input  |
| - go to login     |
+-------------------+
          |
          | writes username/password
          v
+-------------------+
|  credentials.txt  |
|   local storage   |
+-------------------+
          ^
          | reads credentials
          |
+-------------------+
|       Login       |
| - credential check|
| - create session  |
+-------------------+
          |
          | stores session token
          v
+-------------------+
| SharedPreferences |
|   SessionPrefs    |
+-------------------+
          ^
          | reads / clears token
          |
+-------------------+
|      Profile      |
| - authenticated   |
| - logout          |
+-------------------+
```

## Trust boundaries
You can draw two simple trust boundaries:

1. **UI / Activity layer**
   - MainActivity
   - Login
   - Profile

2. **Local storage layer**
   - credentials.txt
   - SharedPreferences (SessionPrefs)

## Main security-sensitive assets to label on the diagram
- Username
- Password
- Stored credentials
- Session token

## Short figure caption
> Figure X. Simple system model of the app, showing user interaction, local credential storage, login verification, session token storage, and the authenticated profile state.
