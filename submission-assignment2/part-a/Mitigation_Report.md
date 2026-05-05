# 5. Mitigation

## 5.1 Enforce HTTPS Only

Firstly, cleartext traffic should be disabled in the `AndroidManifest.xml` file by removing or changing:

```xml
android:usesCleartextTraffic="true"
```

to:

```xml
android:usesCleartextTraffic="false"
```

This ensures that all sensitive communications are conducted over HTTPS. According to Google Android Developer Documentation, disabling this attribute prevents the application from initiating unencrypted network requests, as the platform will block cleartext traffic by default.

Secondly, the HTTP URL in `MainActivity.java` (line 38) should be replaced with an HTTPS endpoint:

```java
webView.loadUrl("https://www.example.com");
```

This prevents the WebView from loading insecure content and reduces exposure to interception attacks.

---

## 5.2 Proper TLS Validation

The insecure implementation in `MainActivity.java` (line 35):

```java
sslErrorHandler.proceed();
```

must be removed and replaced with:

```java
sslErrorHandler.cancel();
```

or by relying on the default system behavior.

According to Google Android Developer Documentation, when an SSL certificate error occurs, the application should always cancel the connection. Continuing despite certificate errors is unsafe because it allows connections to servers with invalid or forged certificates, enabling MITM attacks.

---

## 5.3 Secure Hostname Verification

In `MainActivity.java` (lines 39–42), a `HostnameVerifier` is implemented where the `verify(...)` method always returns `true`.

Although the decompiled code does not clearly show this verifier being attached to an active connection, this pattern is inherently insecure. It suggests that hostname verification may be bypassed, weakening TLS authentication.

This implementation should be removed, and the application should rely on the default hostname verification mechanism provided by the platform. Proper hostname verification ensures that the server’s certificate matches the intended domain, preventing impersonation attacks.

---

## 5.4 Network Security Configuration

Finally, Android Network Security Configuration can be used to enforce stricter security policies, such as restricting cleartext traffic and defining trusted certificates:

```xml
<network-security-config>
    <base-config cleartextTrafficPermitted="false">
        <trust-anchors>
            <certificates src="system" />
        </trust-anchors>
    </base-config>
</network-security-config>
```

This provides centralized control over network security settings and ensures consistent enforcement across the application.

For high-risk scenarios, certificate pinning can also be considered as a defense-in-depth mechanism. This reduces the risk of attacks involving forged certificates or compromised Certificate Authorities.
