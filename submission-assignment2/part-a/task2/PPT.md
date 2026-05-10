# Part A Task 2 — Slide Deck

> 8 slides | ~2 min 30 sec | Speaker: Joey

---

## Slide 1: Task 2 Overview

**Title:** Understand the App & Build a Network-Focused Threat Model

**Bullets:**
- What the app does and what data travels over the network
- Simple system architecture
- Threat model: who can attack and how

---

## Slide 2: What Is This App?

**Title:** App Overview — `com.example.mastg_test0019`

**Bullets:**
- Single-activity WebView browser
- Loads `http://www.example.com` on launch
- No login, no local storage, no authentication
- **But:** every network decision is deliberately insecure

**Visual:** Screenshot of `MainActivity.java` showing `loadUrl("http://www.example.com")`

---

## Slide 3: Network Behavior

**Title:** What Travels Over the Network?

**Table:**

| Step | Action | Protocol | Security |
|------|--------|----------|----------|
| 1 | DNS lookup | UDP/53 | Unencrypted |
| 2 | HTTP GET request | HTTP/80 | **Cleartext** |
| 3 | Server response | HTTP/80 | **Cleartext** |
| 4 | WebView renders | — | No integrity check |

**Key Point:** No TLS. Any node on the path can read or modify this traffic.

---

## Slide 4: System Model

**Title:** System Architecture

**Diagram (ASCII → redraw in PPT):**

```
User → MainActivity → WebView → HTTP GET → Wi-Fi/Cell → Internet → www.example.com
              │           │
              └─ WebViewClient ─┘
                 (ignores SSL errors)
```

**Components:**
- **User:** Trusts the content is legitimate
- **WebView:** Renders remote HTML/JS
- **WebViewClient:** Overrides `onReceivedSslError` → `proceed()`
- **HostnameVerifier:** Returns `true` for **any** hostname
- **Network:** Broadcast medium — attacker can observe

---

## Slide 5: The Attacker

**Title:** On-Path Attacker (MITM)

**Bullets:**
- **Position:** Same Wi-Fi, rogue access point, or compromised router
- **Capabilities:**
  1. **Read** all HTTP traffic (no encryption)
  2. **Modify** HTML/JS before it reaches the WebView
  3. **Impersonate** the server (certificate validation disabled)
- **Skill required:** Low — `mitmproxy`, `Burp Suite`, or even `Wireshark`

**Visual:** Simple MITM diagram (attacker between phone and server)

---

## Slide 6: Attack Scenario 1 — Cleartext Eavesdropping

**Title:** Scenario A: Reading Everything Over HTTP

**Flow:**
```
Phone ──HTTP GET──> [Attacker on same Wi-Fi] ──HTTP GET──> Server
                          │
                          ▼
                    "GET / HTTP/1.1
                     Host: www.example.com"
```

**Impact:** Attacker sees headers, cookies, query parameters, and full response body — no effort required.

---

## Slide 7: Attack Scenario 2 — Certificate & Hostname Bypass

**Title:** Scenario B & C: Even HTTPS Would Not Help

**Two problems side-by-side:**

| Issue | Code Evidence | Result |
|-------|--------------|--------|
| SSL error ignored | `sslErrorHandler.proceed()` | Any fake certificate accepted |
| Hostname bypass | `HostnameVerifier` returns `true` | Wrong server accepted |

**Combined impact:** App thinks it is talking to `www.example.com`, but it could be `attacker.com` — and it would never warn the user.

---

## Slide 8: STRIDE Summary & Trust Boundaries

**Title:** Why This Matters — STRIDE Analysis

**Table:**

| Threat | Applicable? | Why |
|--------|-------------|-----|
| **S**poofing | ✅ Yes | No hostname validation |
| **T**ampering | ✅ Yes | HTTP allows injection |
| **I**nformation Disclosure | ✅ Yes | Cleartext traffic |
| **E**levation of Privilege | ✅ Yes | Injected JS runs in WebView |

**Bottom line:**
- ❌ No **Confidentiality** — no encryption
- ❌ No **Integrity** — no tamper detection
- ❌ No **Authenticity** — no server verification

**Next:** Task 3 shows exactly where in the code this happens.

---

## Speaker Notes (General)

- Keep slides visual — diagrams over paragraphs
- Each slide ~15–20 seconds
- Highlight that the app is *deliberately* vulnerable (OWASP test case), but the patterns are real
- Transition to Task 3: "Now let’s look at the exact code locations..."
