# Task 2 Key Content Summary (English)
## 1. App Function Summary
This Android application requires user authentication via account and password. It features no password plaintext display, no verification code for account registration or login, and does not support password remember or auto-login functions—manual account and password input is mandatory for each login attempt. The login process is instant (one-click quick login) with no obvious waiting for server verification. A generic error prompt *wrong credential* is displayed for all login failures, without distinguishing between invalid account, incorrect password, or other authentication errors. After successful login, users can access the app’s main functional modules, and the login state is not persisted locally (re-login is required after app restart).

## 2. System Model Diagram

```
User → [UI Layer (Login Page / Main Function Pages)] → [Authentication Module] → [Network Module] → Backend Server
       ↖️(Login Failed: Prompt)        ↙️(Login Succeeded)
  UI Layer (Display wrong credential)    UI Layer (Load Main Functions)
```

## 3. Component Description
- **UI Layer**: The visual interaction interface of the app, including a dedicated login page (with only account/password input fields and a login button) and functional pages accessible after successful authentication. It is responsible for receiving user input and feeding back authentication results and functional operation status.
- **Authentication Module**: The core business module for login verification, responsible for encapsulating user-input account and password into verification requests, and forwarding valid requests to the network module. It also receives authentication feedback from the server and triggers corresponding UI prompts for success or failure.
- **Network Module**: The data transmission bridge between the client and server, responsible for sending login verification requests to the backend server and receiving the server’s verification response data, with no complex data processing logic.
- **Backend Server**: The remote server for the app, which persistently stores valid user account and password credentials, executes the core login verification logic, and returns binary verification results (success/failure) to the client network module.
- **Local Storage**: A lightweight local data storage component with no persistent storage of login credentials (e.g., account, password, login token). It only stores temporary runtime data of the app and clears relevant session data after app exit.

## 4. Key Asset Description
Key security assets of the application are focused on the **account authentication link** (the core link related to cryptographic and randomness vulnerability analysis), including two main categories:
- **Primary Sensitive Assets**: User account identifiers (e.g., username/phone number/email) and user login passwords. These are the core protected assets, as their leakage or forgery will directly lead to unauthorized access to the app.
- **Secondary Sensitive Assets**: Login verification request/response data transmitted between the client network module and the backend server. This data contains encapsulated account and password information, and its unencrypted transmission or tampering will pose a direct security risk to the primary assets.

No other persistent sensitive business data is identified in the local storage due to the absence of login state persistence and automatic login functions.

## 5. Data Flow Description
The core data flow of the application is concentrated in the **login authentication process** (the key flow for subsequent vulnerability analysis), with a linear and simplified transmission logic:
1. **Input Stage**: The user enters the account and password in the input fields of the UI layer’s login page, and the UI layer forwards the plaintext input data to the authentication module.
2. **Encapsulation Stage**: The authentication module encapsulates the received account and password into a standard login verification request data packet (without additional verification code or random parameter attachment).
3. **Transmission Stage**: The network module receives the encapsulated request packet and sends it to the backend server through the network for verification.
4. **Verification Stage**: The backend server executes the credential verification logic by matching the received account and password with the locally stored valid credentials, and returns a binary response (success/failure) to the client network module.
5. **Feedback Stage**: The network module forwards the server’s response to the authentication module. If verified successfully, the authentication module triggers the UI layer to load and display the main functional pages; if verified failed, the authentication module instructs the UI layer to display the generic *wrong credential* error prompt.

No additional data interaction or secondary verification occurs during the entire process, and no login-related data is persisted to local storage at any stage.

## 6. Security Assumptions & Analysis Boundaries
### Security Assumptions
1. The app’s instant login feature implies that the login verification logic is lightweight, with no complex server-side interaction or multi-step validation processes; the backend server returns authentication results in real time with minimal latency.
2. The generic *wrong credential* failure prompt prevents account enumeration attacks, as attackers cannot distinguish the root cause of login failure and thus cannot confirm the validity of user accounts.
3. No persistent storage of login credentials in local storage eliminates the risk of credential leakage from local file reading, but the account and password data may be exposed during network transmission (e.g., unencrypted HTTP transmission).
4. The absence of password plaintext display function reduces the risk of shoulder surfing attacks for password input, but the password encryption/hashing mechanism (if any) for request encapsulation is unknown and may have cryptographic vulnerabilities.
5. The app’s network module only transmits login verification requests and response data, with no additional random parameters or session tokens attached during the authentication process.

### Analysis Boundaries
1. **Scope Limitation**: The analysis is focused **only on the login authentication link** of the app, as it is the core link related to cryptographic and randomness vulnerabilities (the only assessed vulnerability class for this assignment). Non-authentication functional modules and irrelevant business logic are excluded from the analysis scope.
2. **Attacker Boundary**: The analysis assumes attackers have **client-side access capabilities** (e.g., decompile the APK, intercept client-side network traffic, simulate manual login attempts on the emulator) but no direct access to the backend server (cannot modify server-side credential data or verification logic).
3. **Data Boundary**: The analysis only covers account/password data and login request/response data; no other business data, app runtime logs, or non-sensitive local temporary data are included in the security analysis.
4. **Vulnerability Boundary**: The analysis is restricted to vulnerabilities related to **cryptographic misuse and insecure randomness** (e.g., weak password encryption, unencrypted data transmission, lack of random salt in hashing). Other vulnerability classes (e.g., UI design flaws, functional bugs) are out of the analysis scope for this assignment.
