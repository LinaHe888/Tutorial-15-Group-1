# Assignment 2 Part A Report Draft

## System and Threat Model
The provided APK (`a2_case1.apk`) is an Android application with package name `com.example.mastg_test0019`. Its main activity creates a `WebView` and loads remote web content. The main protected asset for Part A is the confidentiality and integrity of the network content loaded by the app. The key trust boundary is between the Android device and the remote web origin contacted by the application.

A realistic attacker is an on-path adversary, such as a malicious public Wi-Fi operator or local network attacker, who can observe, intercept, or modify network traffic between the device and the remote server. This attacker model directly matches the Assignment 2 Part A scope, which focuses on insecure network transport and TLS validation weaknesses that can enable man-in-the-middle attacks.

## Vulnerability Discovery and Impact Reasoning
The strongest Part A finding is that the app allows and actively uses cleartext HTTP traffic. Static analysis of `AndroidManifest.xml` shows that the application sets `android:usesCleartextTraffic="true"`. This configuration explicitly permits unencrypted HTTP communication. In addition, `MainActivity.java` contains `webView.loadUrl("http://www.example.com")`, showing that the application does not merely allow cleartext transport in configuration but actually uses it at runtime.

This creates a clear man-in-the-middle risk. Because the content is loaded over HTTP rather than HTTPS, an on-path attacker can read, block, or modify the traffic without needing to break TLS. Any web content rendered inside the app can therefore be spoofed or tampered with in transit, undermining both confidentiality and integrity.

A supporting weakness is the app’s handling of TLS errors in `WebView`. In `onReceivedSslError(...)`, the app calls `sslErrorHandler.proceed()`, meaning certificate validation failures are ignored. This further weakens transport security because an attacker-controlled or invalid certificate would not necessarily stop content from loading. The code also contains a permissive `HostnameVerifier` pattern, although the current evidence does not yet confirm active runtime use. For this reason, the primary finding is best framed as insecure cleartext transport, with broken SSL handling as supporting evidence.

## Mitigation
The root cause should be fixed in two ways. First, cleartext traffic should be disabled by removing or reversing `android:usesCleartextTraffic="true"`, and all remote content should be loaded over HTTPS only. Second, TLS errors in `WebView` should not be ignored: `sslErrorHandler.proceed()` should be removed and replaced with secure default handling that blocks invalid certificates. If hostname verification is used for custom HTTPS clients, the app must rely on the platform-default verifier rather than an always-true implementation.

These mitigations work because they restore the essential security properties required for transport security. Disallowing HTTP prevents passive observation and active tampering over cleartext channels, while proper certificate and hostname validation ensures that the app only trusts authentic servers during TLS connections.
