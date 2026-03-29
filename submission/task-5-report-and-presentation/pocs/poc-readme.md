# README.md

## PoC: Insecure Random Number Generation (MASTG-TEST-0016)

### 1. Project Overview
This Proof of Concept (PoC) demonstrates a vulnerability in an Android application (MASTG-TEST-0016) where session tokens are generated using an insecure Pseudo-Random Number Generator (PRNG). By collecting a sequence of generated tokens, an attacker can determine the internal state of the generator and predict future tokens, leading to **Session Hijacking**.

* **Vulnerability Type:** CWE-330: Use of Insufficiently Random Values
* **Test Case:** MASTG-TEST-0016 (Based on OWASP MASTG)
* **Root Cause:** Use of `java.util.Random` instead of a cryptographically secure alternative.

---

### 2. Machine Specifications & Environment Setup
To successfully reproduce this PoC, the following environment is required:

#### Hardware Specifications
* **Processor:** x86_64 or ARM64 (Compatible with Android Virtual Devices)
* **Memory:** Minimum 8GB RAM (16GB recommended for smooth emulation)

#### Software Dependencies
* **Operating System:** macOS (as shown in the PoC video), Linux, or Windows.
* **Android SDK Platform-Tools:** Specifically `adb` (Android Debug Bridge).
* **Android Emulator:** 
    * **Target:** Android 10 (API 29) or higher.
    * **Image:** Google APIs image with `root` access enabled.
* **Terminal Emulator:** zsh, bash, or similar.

---

### 3. Installation & Configuration
1.  **Start the Emulator:** Launch your AVD (Android Virtual Device) via Android Studio.
2.  **Install the APK:**
    ```bash
    adb install MASTG-TEST-0016.apk
    ```
3.  **Gain Root Access:** The PoC requires reading private application data.
    ```bash
    adb root
    ```

---

### 4. Reproduction Steps (PoC Walkthrough)

#### Step 1: Token Generation
1.  Open the **MASTG-TEST-0016** app on the emulator.
2.  Navigate to the **Register** screen.
3.  Enter a dummy username and password (e.g., `123`), then click **Register**.
4.  The app transitions to the **Login** screen. Enter the credentials and click **Login**.

#### Step 2: Data Extraction
1.  Once logged in, the app generates a `sessionToken` and saves it to a shared preference file.
2.  In your terminal, execute the following command to extract the token:
    ```bash
    adb shell cat /data/data/com.example.mastg.test0016/shared_prefs/SessionPrefs.xml
    ```
3.  Identify the string value for the key `sessionToken` (e.g., `QnidCjrq5doa5Drh/`).

#### Step 3: Pattern Collection
1.  Click **Log out** in the app.
2.  Repeat the Login/Extraction process 5-10 times.
3.  Record each token in a sequential list (as shown in the video's yellow sticky note).

#### Step 4: Prediction
1.  Input the collected tokens into a PRNG cracker or analysis script.
2.  **Expected Result:** Because `java.util.Random` is a Linear Congruential Generator (LCG), the next token in the sequence can be mathematically predicted with high accuracy.

---

### 5. Video Demonstration Summary
The provided video (`poc-video.mp4`) illustrates the following:
* **00:00 - 00:05:** Introduction to the test case and token constraints (16 characters).
* **00:06 - 00:45:** The iterative process of logging in, dumping the `SessionPrefs.xml` via ADB, and copying tokens to a notepad.
* **00:46 - 01:05:** Comparison of multiple tokens (e.g., `MmCGQ7ZJiXDoG8ut/`, `b9JOcSUilGqxF0bC/`) to confirm the predictable nature of the sequence.

---

### 6. Remediation
To fix this vulnerability, replace `java.util.Random` with `java.security.SecureRandom`, which provides a non-deterministic, cryptographically strong PRNG.
