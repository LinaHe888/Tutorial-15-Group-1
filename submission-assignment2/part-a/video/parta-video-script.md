# Part A Video Script

## Target length
- 45 to 60 seconds for the Part A segment itself
- keep the final recorded Part A presentation within the assignment time limit

## Script
Hi, I’m presenting Assignment 2 Part A. The app under analysis is `a2_case1.apk`, package `com.example.mastg_test0019`. Its main protected asset is the integrity and confidentiality of web content loaded inside the app, and the relevant attacker is an on-path adversary such as a malicious Wi-Fi operator.

The primary vulnerability is insecure network transport. In `AndroidManifest.xml`, the app sets `android:usesCleartextTraffic="true"`, which allows HTTP traffic. In `MainActivity.java`, the `WebView` explicitly loads `http://www.example.com`, showing that cleartext transport is not only allowed but actually used.

This means an attacker on the network can intercept or modify the content loaded by the app, creating a clear man-in-the-middle risk. A supporting issue is that the app also ignores TLS certificate errors in `onReceivedSslError(...)` by calling `proceed()`.

The fix is to disable cleartext traffic, require HTTPS, and stop ignoring SSL errors so that invalid certificates are rejected.
