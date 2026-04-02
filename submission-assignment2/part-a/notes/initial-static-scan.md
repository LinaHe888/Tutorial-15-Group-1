# Part A Initial Static Scan

## Scope Reminder
According to `assignment2-spec-2.pdf`, Part A is graded only on insecure network transport / TLS configuration issues that could enable man-in-the-middle attack, such as:
- cleartext traffic
- broken certificate validation
- broken hostname validation

The analysis below intentionally focuses only on that scope.

## APK Under Analysis
- APK: `a2_case1.apk`
- Package: `com.example.mastg_test0019`
- Main activity: `com.example.mastg_test0019.MainActivity`

## Initial Findings
### Finding candidate 1 — Cleartext traffic explicitly enabled
From `AndroidManifest.xml`:
```xml
<application
    ...
    android:usesCleartextTraffic="true"
    ...>
```
This indicates that the app allows cleartext HTTP traffic.

### Finding candidate 2 — WebView loads an HTTP URL
From `MainActivity.java`:
```java
webView.loadUrl("http://www.example.com");
```
This shows actual use of cleartext traffic in the app logic, not just permissive manifest configuration.

### Finding candidate 3 — SSL errors in WebView are ignored
From `MainActivity.java`:
```java
@Override
public void onReceivedSslError(WebView webView2, SslErrorHandler sslErrorHandler, SslError sslError) {
    sslErrorHandler.proceed();
}
```
This indicates the WebView continues loading even when SSL certificate validation fails.

### Finding candidate 4 — Permissive hostname verifier pattern present
From `MainActivity.java`:
```java
new HostnameVerifier() {
    @Override
    public boolean verify(String str, SSLSession sSLSession) {
        return true;
    }
};
```
This is a dangerous pattern, but at this stage it is only known to be instantiated. It has not yet been confirmed as actively attached to an HTTPS client. Therefore it should currently be treated as a supporting clue rather than the primary Part A finding.

## Current Best Primary Vulnerability Candidates
Strictly under the Part A rubric, the strongest candidates are:
1. **Cleartext traffic enabled and used**
2. **WebView ignores SSL certificate errors**

Both fit the intended Part A vulnerability class and support a realistic MITM threat model.

## Recommended Next Step
Perform dynamic validation to determine which finding should be treated as the main Part A vulnerability:
- test whether the app actually uses HTTP traffic in runtime
- test whether a proxy / MITM setup can exploit the permissive SSL handling in WebView
