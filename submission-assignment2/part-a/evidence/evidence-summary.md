# Part A Evidence Summary

## Primary Finding
**Insecure network transport via cleartext HTTP enabled and used**

## Primary Evidence
- `AndroidManifest.xml`: `android:usesCleartextTraffic="true"`
- `MainActivity.java`: `webView.loadUrl("http://www.example.com")`

## Supporting Evidence
- `MainActivity.java`: `onReceivedSslError(...){ proceed(); }`
- `MainActivity.java`: permissive `HostnameVerifier` pattern

## Interpretation
The primary Part A finding should be framed as **cleartext traffic enabled and used**. The SSL handling and hostname verifier code strengthen the case that the app’s network trust model is weak, but are currently supporting evidence rather than the main finding.

## Reference
See `code-evidence.md` for the full code snippets.
