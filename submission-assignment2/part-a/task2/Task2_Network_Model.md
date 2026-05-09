# Task 2: App Understanding and Network-Focused Threat Model

## 1. App Overview

### 1.1 Application Identity
- **Package Name:** `com.example.mastg_test0019`
- **App Label:** `MASTGTEST0019` (from manifest `android:label`)
- **Target SDK:** Android 14 (API 34)
- **Purpose:** This is a deliberately vulnerable test application from the OWASP MASVS/MSTG framework (MASTG-TEST-0019), designed to demonstrate insecure network communication patterns in Android apps.

### 1.2 App Functionality
The application consists of a **single activity** (`MainActivity.java`) that performs one primary function:

1. **Renders a full-screen WebView** (`activity_main.xml` layout containing `R.id.webview`)
2. **Loads an external website** via `webView.loadUrl("http://www.example.com")`
3. **Uses a custom WebViewClient** that overrides SSL error handling to always proceed (`sslErrorHandler.proceed()`)
4. **Implements a permissive HostnameVerifier** that unconditionally returns `true` for any hostname

In practical terms, this is a **minimal web browser app** that embeds a single web page. It has no local data storage, no user authentication flows, no API calls, and no complex business logic — its entire behavior is centered on fetching and displaying web content over the network.

### 1.3 Network Behavior Summary

When the app launches, the following network activity occurs:

| Step | Action | Protocol | Security Status |
|------|--------|----------|----------------|
| 1 | App resolves `www.example.com` via DNS | UDP/53 | Unencrypted, standard |
| 2 | App sends HTTP GET to `www.example.com` | HTTP/80 | **Cleartext** — no TLS encryption |
| 3 | Server returns HTML/JS/CSS content | HTTP/80 | **Cleartext** — no confidentiality/integrity |
| 4 | WebView renders received content | N/A | Content is displayed to user |

Because the URL is hardcoded to `http://` (not `https://`), and the manifest explicitly permits cleartext traffic (`android:usesCleartextTraffic="true"`), **all communication occurs over unencrypted HTTP**.

If the server were to redirect to HTTPS or if the URL were changed to `https://`, the custom WebViewClient would still bypass certificate validation (`sslErrorHandler.proceed()`), and the HostnameVerifier would accept any certificate regardless of domain match.

### 1.4 Data Traveling Over the Network
Given the app's minimal design, the network traffic includes:

- **Outbound:** HTTP GET request headers (User-Agent, Accept, etc.)
- **Inbound:** HTML page content, JavaScript, CSS, images, or any other web resources from `www.example.com`
- **Implicit metadata:** Source IP, destination IP, DNS queries, timing patterns

In a real-world app with similar architecture, this traffic might also include:
- User credentials or session tokens (if login forms are rendered)
- Personal user data entered into web forms
- Cookies or local storage sync
- API calls from embedded JavaScript

---

## 2. System Model

### 2.1 Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                      Android Device                         │
│  ┌─────────────────────────────────────────────────────┐  │
│  │  ┌─────────────┐                                    │  │
│  │  │  User       │─── taps/opens app                  │  │
│  │  └─────────────┘                                    │  │
│  │         │                                           │  │
│  │         ▼                                           │  │
│  │  ┌─────────────────────────┐                        │  │
│  │  │   MainActivity.java    │                        │  │
│  │  │  ┌─────────────────┐  │                        │  │
│  │  │  │  WebView        │  │                        │  │
│  │  │  │  (R.id.webview) │  │                        │  │
│  │  │  └─────────────────┘  │                        │  │
│  │  │         │             │                        │  │
│  │  │  ┌──────┴──────┐    │                        │  │
│  │  │  │ WebViewClient│    │                        │  │
│  │  │  │  - ignores   │    │                        │  │
│  │  │  │    SSL errors│    │                        │  │
│  │  │  └─────────────┘    │                        │  │
│  │  └──────────┬──────────┘                        │  │
│  │             │  HTTP GET http://www.example.com    │  │
│  │             │  (cleartext, no cert validation)    │  │
│  └─────────────┼────────────────────────────────────┘  │
│                │                                           │
│         ┌──────┴──────┐                                  │
│         │  OS Network   │                                │
│         │  Stack        │                                │
│         │  (Wi-Fi /     │                                │
│         │   Cellular)   │                                │
│         └──────┬──────┘                                  │
└────────────────┼──────────────────────────────────────────┘
                 │
                 ▼ HTTP/80 (cleartext)
┌──────────────────────────────────────┐
│         Network / Internet             │
│  ┌────────────────────────────────┐  │
│  │  DNS Resolution (UDP/53)         │  │
│  │  Router / ISP Infrastructure    │  │
│  └────────────────────────────────┘  │
│                │                      │
│                ▼ HTTP/80              │
│  ┌────────────────────────────────┐  │
│  │  Target Server                 │  │
│  │  www.example.com:80            │  │
│  │  (or attacker-controlled)      │  │
│  │  Returns HTML/JS/CSS content   │  │
│  └────────────────────────────────┘  │
└──────────────────────────────────────┘
```

### 2.2 Components Description

| Component | Role | Security Relevance |
|-----------|------|-------------------|
| **User** | Opens the app and views web content | Trusts that content is legitimate and connection is secure |
| **MainActivity** | Single Android activity hosting the WebView | Contains insecure configuration code (HTTP URL, SSL bypass, hostname bypass) |
| **WebView** | Renders web content from remote URL | Displays attacker-controlled content if MITM is active |
| **WebViewClient** | Handles WebView callbacks including SSL errors | Overrides `onReceivedSslError` to **ignore** certificate validation |
| **HostnameVerifier** | Validates server hostname against certificate | Custom implementation returns `true` for **any** hostname |
| **Android OS Network Stack** | Manages network connectivity | Permits cleartext traffic due to manifest flag |
| **Wi-Fi / Cellular Network** | Transport layer | Broadcast medium — any on-path node can observe traffic |
| **www.example.com** | Destination server | Intended source of content, but app cannot verify its identity |

---

## 3. Threat Model

### 3.1 Threat Model Diagram (STRIDE-style)

```
                                ┌─────────────────┐
                                │   Attacker      │
                                │ (On-path MITM)  │
                                │                 │
                                │ ┌───────────┐ │
                                │ │ Same Wi-Fi │ │
                                │ │  Rogue AP  │ │
                                │ │   / Proxy  │ │
                                │ └───────────┘ │
                                └───────┬───────┘
                                        │
        ┌───────────────────────────────┼───────────────────────────────┐
        │                               │                               │
        ▼                               ▼                               ▼
┌───────────────┐              ┌───────────────┐              ┌───────────────┐
│   Spoofing    │              │  Interception │              │   Tampering   │
│  (Pretend to  │              │  (Read        │              │  (Modify      │
│   be server)  │              │   traffic)    │              │   content)    │
└───────────────┘              └───────────────┘              └───────────────┘
        │                               │                               │
        └───────────────┬───────────────┴───────────────┬───────────────┘
                        │                               │
                        ▼                               ▼
              ┌─────────────────┐               ┌─────────────────┐
              │  User's Device  │               │  User's Device  │
              │  (WebView)      │◄──────────────│  (WebView)      │
              │  Thinks it is   │  Fake Content │  Receives       │
              │  talking to     │               │  modified HTML/ │
              │  example.com    │               │  JS with malware│
              └─────────────────┘               └─────────────────┘
```

### 3.2 Attackers

#### Primary Threat: On-Path Network Attacker (MITM)
- **Position:** Same Wi-Fi network, rogue access point, compromised router, or ISP-level interception
- **Capabilities:**
  - **Read** all HTTP traffic in cleartext (no encryption)
  - **Modify** HTML/JS/CSS responses before they reach the WebView
  - **Impersonate** `www.example.com` by presenting a fake certificate (bypassed by `sslErrorHandler.proceed()`)
  - **Redirect** traffic to attacker-controlled servers (bypassed by permissive HostnameVerifier)
- **Skill Level:** Low — basic tools like `mitmproxy`, `Burp Suite`, or `Wireshark` are sufficient

### 3.3 Attack Scenarios

#### Scenario A: Cleartext Eavesdropping (HTTP)
```
[User's Phone] ──HTTP GET──> [Router] ──HTTP GET──> [www.example.com]
                                     │
                                     ▼
                               [Attacker sniffs]
                               "GET / HTTP/1.1\nHost: www.example.com\n..."
```
- Because traffic is HTTP (port 80), it is entirely unencrypted
- Attacker can read request headers, query parameters, cookies, and full response body
- No technical barriers prevent interception

#### Scenario B: HTTPS Downgrade / Certificate Impersonation
```
[User's Phone] ──HTTPS──> [Attacker Proxy] ──HTTPS──> [www.example.com]
                  │              │
                  │              ▼
                  │        [Fake cert presented]
                  │        [sslErrorHandler.proceed() → accepted]
                  ▼
           [App continues, thinking connection is secure]
```
- Even if the URL were HTTPS, the app would accept any certificate
- Attacker can present a self-signed certificate for `www.example.com`
- The WebViewClient's `onReceivedSslError()` callback calls `proceed()`, suppressing the security warning

#### Scenario C: Hostname Spoofing
```
[User's Phone] ──TLS──> [attacker.com] (cert for attacker.com)
                              │
                              ▼
                    [HostnameVerifier.verify("www.example.com", ...)]
                    → returns TRUE (always)
                    → App accepts wrong server
```
- The custom HostnameVerifier returns `true` regardless of hostname mismatch
- App connects to `attacker.com` but thinks it is `www.example.com`
- Attacker can serve malicious content under a trusted domain name in the app's context

### 3.4 STRIDE Analysis

| Threat | Description | Applicable? | Evidence |
|--------|-------------|-------------|----------|
| **S**poofing | Attacker impersonates `www.example.com` | ✅ Yes | HostnameVerifier returns true; SSL errors ignored |
| **T**ampering | Attacker modifies content in transit | ✅ Yes | HTTP cleartext allows injection; HTTPS validation bypassed |
| **R**epudiation | App cannot prove content came from legitimate server | ✅ Yes | No certificate pinning or integrity checks |
| **I**nformation Disclosure | Attacker reads transmitted data | ✅ Yes | HTTP = cleartext; HTTPS validation disabled |
| **D**enial of Service | Attacker drops or blocks traffic | ⚠️ Partial | MITM can drop packets; not specific to app vulnerability |
| **E**levation of Privilege | Attacker gains control via injected content | ✅ Yes | WebView executes attacker-controlled JavaScript |

---

## 4. Data Flow Analysis

### 4.1 Network Data Flow Diagram

```
┌─────────────────┐
│   User opens    │
│      app        │
└────────┬────────┘
         │
         ▼
┌─────────────────┐     ┌─────────────────┐
│  MainActivity   │────▶│   DNS Query     │
│   onCreate()    │     │ www.example.com │
└────────┬────────┘     └────────┬────────┘
         │                       │
         │    ┌──────────────────┘
         │    │
         ▼    ▼
┌─────────────────────────────┐
│     HTTP GET Request        │
│  GET / HTTP/1.1             │
│  Host: www.example.com      │
│  User-Agent: ...            │
│  (NO encryption — HTTP/80)  │
└─────────────┬───────────────┘
              │
              ▼
    ┌─────────────────┐
    │  Network Path   │
    │  (Wi-Fi/cell)   │
    │  ┌───────────┐  │
    │  │  Attacker │  │◄── can read/modify/redirect
    │  │  (MITM)   │  │
    │  └───────────┘  │
    └────────┬────────┘
             │
             ▼
┌─────────────────────────────┐
│     HTTP Response           │
│  HTTP/1.1 200 OK            │
│  Content-Type: text/html    │
│  <html>... (modified?) ...  │
│  (NO integrity protection)  │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│      WebView renders        │
│  attacker-modified content  │
│  (phishing, malware, etc.)  │
└─────────────────────────────┘
```

### 4.2 Trust Boundaries

| Boundary | Left Side | Right Side | Trust Issue |
|----------|-----------|------------|-------------|
| App ↔ OS | App code | Android network stack | App disables OS security (cleartext flag, SSL bypass) |
| OS ↔ Network | Device | Wi-Fi / Internet | No encryption — trust depends on physical security of network |
| Network ↔ Server | Internet | `www.example.com` | No authentication — app cannot verify it reached the real server |
| Server ↔ Content | Server | Response HTML | No integrity check — content can be modified in transit |

---

## 5. Summary

This application is a **minimal WebView-based browser** with a deliberately insecure network configuration. Its sole network operation — loading `http://www.example.com` — is performed over **unencrypted HTTP** with **all TLS validation mechanisms disabled**.

The threat model identifies a **network-path attacker** (MITM) as the primary threat, with three concrete attack paths:
1. **Eavesdropping** on cleartext HTTP traffic
2. **Certificate impersonation** bypassed by `sslErrorHandler.proceed()`
3. **Hostname spoofing** accepted by the permissive `HostnameVerifier`

These vulnerabilities directly violate the three core security guarantees of network communication:
- ❌ **Confidentiality** — no encryption
- ❌ **Integrity** — no tamper detection
- ❌ **Authenticity** — no server verification

In a real-world deployment, this architecture would expose users to phishing, credential theft, session hijacking, and malicious content injection — all from a passive or active attacker on the same network path.
