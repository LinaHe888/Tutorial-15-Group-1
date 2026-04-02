# Part A Code Evidence

## Manifest Evidence
### Cleartext traffic enabled
File: `AndroidManifest.xml`

```xml
<application
    ...
    android:usesCleartextTraffic="true"
    ...>
```

### Why it matters
This explicitly allows cleartext HTTP traffic, which weakens transport security and enables network interception or modification by an on-path attacker.

---

## MainActivity Evidence
### WebView loads HTTP content
File: `MainActivity.java`

```java
webView.loadUrl("http://www.example.com");
```

### Why it matters
This shows that the app does not merely allow cleartext traffic in configuration; it actually uses an HTTP URL at runtime.

---

### WebView proceeds on SSL errors
File: `MainActivity.java`

```java
@Override
public void onReceivedSslError(WebView webView2, SslErrorHandler sslErrorHandler, SslError sslError) {
    sslErrorHandler.proceed();
}
```

### Why it matters
This disables the normal security reaction to certificate validation failures in a `WebView`. An attacker presenting an invalid or attacker-controlled certificate may still be able to load content in the app.

---

### Permissive hostname verifier pattern
File: `MainActivity.java`

```java
new HostnameVerifier() {
    @Override
    public boolean verify(String str, SSLSession sSLSession) {
        return true;
    }
};
```

### Why it matters
This is an insecure hostname verification pattern. However, the current evidence does not yet show that it is actively connected to a live HTTPS client. It should therefore be treated as supporting evidence unless dynamic or additional static proof shows actual use.
