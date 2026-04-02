# Part A Dynamic Validation Plan

## Goal
Strictly validate the Part A finding using runtime evidence that matches the intended rubric scope.

## Validation Targets
### Option A — Cleartext traffic
Objective:
- confirm that the app performs HTTP traffic in runtime
- capture the request in a proxy or network log

Evidence to collect:
- app launch screenshot
- proxy capture showing `http://www.example.com` or equivalent HTTP traffic
- screenshot showing the loaded page in the WebView

### Option B — Broken TLS validation in WebView
Objective:
- confirm that the app proceeds despite certificate errors
- demonstrate that a MITM setup can still load content because `onReceivedSslError(...){ proceed(); }`

Evidence to collect:
- app configured to route through a controlled proxy / MITM environment
- invalid or interception certificate presented to the app
- runtime screenshot showing content still loads despite SSL error condition

## Decision Rule
After dynamic testing, choose the stronger primary finding based on:
- clearer exploitability under an on-path attacker model
- stronger runtime evidence
- simpler and more defensible explanation under the Part A rubric
