# 3. Network Vulnerability: Insecure Transport Configuration Enabling MITM

## Overview

The application suffers from a single vulnerability class: **insecure transport configuration**, which breaks both encryption and TLS validation guarantees. This allows a network attacker to perform **man-in-the-middle (MITM) attacks**, compromising confidentiality and integrity of transmitted data.

This vulnerability is evidenced through multiple code paths that collectively weaken secure communication.

---

## (1) Identification of Insecure Transport Paths (Static Analysis)

The following insecure patterns were identified:

### Cleartext Traffic Enabled (Manifest)

- **File:** AndroidManifest.xml  
- **Code:**  
  `android:usesCleartextTraffic="true"`

- Allows the app to send network requests over HTTP instead of HTTPS.

---

### HTTP Usage in WebView

- **File:** MainActivity.java  
- **Code:**  
  `webView.loadUrl("http://www.example.com");`

- The app explicitly loads a cleartext (HTTP) endpoint.

---

### TLS Validation Bypass in WebView

- **File:** MainActivity.java  
- **Code:**  
  `sslErrorHandler.proceed();`

- Ignores SSL errors and continues the connection even if the certificate is invalid.

---

### Hostname Verification Bypass Pattern

- **File:** MainActivity.java  
- **Code:**  
```java
new HostnameVerifier() { // from class: com.example.mastg_test0019.MainActivity.2
    @Override // javax.net.ssl.HostnameVerifier
    public boolean verify(String str, SSLSession sSLSession) {
        return true;
    }
};
```

- Disables hostname verification by always returning true.

Note: This verifier is instantiated but not clearly attached to a connection. However, it represents an insecure pattern consistent with TLS bypass behavior.

---

## (2) Evidence of Insecure Network Behavior

The vulnerability is supported by the following evidence:

- Manifest explicitly permits cleartext traffic  
- Application actively loads an HTTP endpoint  
- WebView disables SSL certificate validation  
- Hostname verification logic is weakened  

Together, these confirm that **secure transport protections are not enforced**.

---

## (3) Attacker Capabilities

An attacker positioned on the network (e.g., same Wi-Fi, proxy, or ISP path) can:

- Read transmitted data when HTTP is used  
- Intercept HTTPS connections by presenting a fake certificate  
- Bypass certificate validation due to ignored SSL errors  
- Impersonate the server due to disabled hostname verification  

As a result, the attacker can observe, modify, or inject network traffic transparently.

---

## (4) Why This is Unsafe (MITM Relevance)

This configuration breaks the core guarantees of secure communication:

- Confidentiality is lost → attackers can read data  
- Integrity is lost → attackers can modify data  
- Authenticity is lost → server identity is not verified  

These conditions directly enable man-in-the-middle (MITM) attacks.

---

## (5) Realistic Worst-Case Impact

Depending on the data transmitted, the impact may include:

- Credential theft (passwords, tokens, API keys)  
- Session hijacking (via stolen cookies or bearer tokens)  
- Sensitive data exposure (user data, identifiers, app content)  
- Content injection (malicious HTML/JavaScript in WebView)  
- Application behavior manipulation (tampered API responses or redirects)  

In the worst case, an attacker can achieve full compromise of confidentiality and integrity of all network-delivered data.

---

## (6) Example Attack Scenario

On a public Wi-Fi network, an attacker intercepts the app’s request to http://www.example.com and modifies the response to inject malicious content.  
If HTTPS is used, the attacker presents a fake certificate, and due to disabled SSL validation, the app accepts the connection and communicates with the attacker-controlled server.

---

## (7) Attack Flow Summary

An attacker on the same network intercepts the app’s request.  
If HTTP is used, the attacker directly reads and modifies traffic.  
If HTTPS is used, the attacker presents a fake certificate, and due to disabled TLS validation, the app accepts it.  

In both cases, the attacker effectively becomes the server, gaining full control over the communication channel.

---

## Conclusion

The application is vulnerable to MITM attacks because it allows cleartext communication and disables TLS validation, enabling attackers to intercept and manipulate network traffic.