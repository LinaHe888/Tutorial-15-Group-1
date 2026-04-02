# Task 2 - Understand the App and Build a Network-Focused Model

## Goal
Summarise what the app does, what information appears to travel over the network, and build a simple system/threat model focused on on-path attackers.

## App Summary
The provided APK is an Android app that opens a `WebView` in `MainActivity` and loads remote web content. The app requests Internet access and displays external content inside the app interface.

## What Appears to Travel Over the Network
Based on static analysis, the app loads remote web content into a `WebView`. This indicates that the content rendered by the app travels over the network and is exposed to transport-security weaknesses if the channel is not properly protected.

## Relevant Components
- Android app (`com.example.mastg_test0019`)
- `MainActivity`
- `WebView`
- remote web server
- network path between device and server

## Protected Assets
- confidentiality of content loaded by the app
- integrity of content rendered in the app
- authenticity of the remote server or content source

## Threat Model
A realistic attacker is an on-path adversary, such as someone controlling or monitoring the same Wi-Fi network. This attacker may be able to read, intercept, or modify traffic if the app uses insecure transport or weak TLS validation.

## Why This Fits Part A
Part A is specifically scoped to MITM-relevant transport weaknesses. The app’s trust boundary is the connection between the device and the remote content source, so insecure HTTP or broken TLS handling directly affects the app’s security model.

## Outcome
Task 2 is complete at the static-analysis level. Dynamic runtime observation can later be added as supporting evidence if emulator or proxy tooling is available.
