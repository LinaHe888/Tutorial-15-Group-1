# Part A Evidence Summary

## Primary Finding
**Insecure network transport via cleartext HTTP enabled and used**

## Primary Evidence
### 1. Manifest allows cleartext traffic
File: `AndroidManifest.xml`
```xml
android:usesCleartextTraffic="true"
```

### 2. App actually loads an HTTP URL
File: `MainActivity.java`
```java
webView.loadUrl("http://www.example.com");
```

## Supporting Evidence
### 3. TLS certificate errors are ignored in WebView
File: `MainActivity.java`
```java
public void onReceivedSslError(WebView webView2, SslErrorHandler sslErrorHandler, SslError sslError) {
    sslErrorHandler.proceed();
}
```

### 4. Permissive hostname verifier pattern appears in code
File: `MainActivity.java`
```java
public boolean verify(String str, SSLSession sSLSession) {
    return true;
}
```

## Interpretation
The Part A primary vulnerability should be framed around cleartext traffic because it is directly configured and directly used. The SSL handling and permissive hostname verifier code strengthen the argument that the application’s network trust model is insecure, but the hostname verifier is currently better treated as supporting evidence unless runtime use is further demonstrated.
