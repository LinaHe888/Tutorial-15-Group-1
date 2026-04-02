# Part A System and Threat Model Draft

## System Overview
The provided APK is an Android application that displays web content through a `WebView`. The app requests Internet access and loads remote content in the main activity.

## Relevant Components
- `MainActivity`
- `WebView`
- Android application manifest
- Device network connection
- Remote web origin contacted by the app

## Protected Assets
For Part A, the key protected assets are:
- confidentiality and integrity of network traffic loaded by the app
- authenticity of remote content shown inside the app
- user trust in the connection between the app and the remote server

## Trust Boundaries
The main trust boundary is between:
- the Android app / device, and
- the network / remote server

This is especially important because Part A focuses on transport security and MITM-relevant weaknesses.

## Realistic Attacker Model
A realistic attacker is an on-path adversary who can intercept or modify network traffic, for example on:
- public Wi-Fi
- a hostile local network
- a controlled proxy / MITM environment

The attacker does not need local code execution on the device. The attacker’s advantage comes from being able to observe or tamper with insecure transport or improperly validated TLS connections.

## Why This Model Fits the Rubric
This attacker model directly matches the Part A grading scope because the assignment explicitly targets:
- insecure network transport
- certificate validation failures
- hostname validation failures

## Main Security Question
Can the attacker intercept, modify, or spoof the app’s network traffic because the app:
- allows cleartext HTTP, or
- accepts invalid TLS certificates / hostnames?
