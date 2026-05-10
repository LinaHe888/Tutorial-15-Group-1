# Part A Task 2 — Video Script (40 seconds)

> Speaker: Joey  
> Topic: System & Threat Model (Slides 4–6)  
> Target: ~40 sec | ~110 words

---

## Script

"For Task 2, we built a network-focused system and threat model.

The app is a minimal WebView browser. It loads `http://www.example.com` over cleartext HTTP, and even if it used HTTPS, two pieces of custom code disable all TLS protection.

First, the `WebViewClient` overrides `onReceivedSslError` and calls `proceed()` — so any invalid certificate is silently accepted. Second, the `HostnameVerifier` returns `true` for every hostname, so the app never checks if it reached the real server.

Our threat model focuses on an **on-path attacker** — someone on the same Wi-Fi or running a rogue access point. Because traffic is HTTP, the attacker can **read** everything without effort. Because certificate validation is disabled, the attacker can also **modify** responses or **impersonate** the server entirely.

In short: there is no confidentiality, no integrity, and no authenticity."

---

## Timing Breakdown

| Segment | Time | Words |
|---------|------|-------|
| Intro (Task 2 purpose) | ~5 sec | 12 |
| App behavior + two code problems | ~15 sec | 45 |
| Attacker model + three capabilities | ~15 sec | 40 |
| Summary punchline | ~5 sec | 10 |
| **Total** | **~40 sec** | **~107** |

---

## Delivery Tips

- Speak clearly; 40 seconds is tight — do not rush
- Emphasize "cleartext HTTP" and "proceed()" / "returns true" as the core mistakes
- Gesture toward the MITM diagram if one is on screen
- End with the three Ns: "no confidentiality, no integrity, no authenticity" — memorable closing
