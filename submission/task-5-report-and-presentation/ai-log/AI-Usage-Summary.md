# AI Usage Log

This file summarises how AI tools were used in this assignment. Summaries are provided instead of full prompt-response transcripts. The complete prompt–response transcripts for the LLM-based mock Q&A are included below.

## Task 1
Tools Used Codex:
- Used AI to identify suitable tools for APK analysis (apktool and jadx)
- Used AI to understand the decompilation workflow and tool purposes
- Used Codex to confirm that the APK was successfully unpacked and key files were accessible
- Used AI to help locate key components:
- package name
- AndroidManifest.xml
- MainActivity
- Login.java


## Task 2



## Task 3



## Task 4



## Task 5

AI assistance was used to:

- converting content into LaTeX formatted code;
- suggesting how to record the PoC videos;
- helping to understand key concepts;
- conducting mock Q&A;
- helping identify methods for obtaining login tokens and generating related scripts;

## Human Verification and Limits

All AI-generated wording was manually reviewed and edited before use. AI outputs were treated as drafting assistance only. Decompiled code, screenshots, notes, and assignment artefacts were checked manually to confirm that explanations matched the actual application behaviour. Final conclusions about vulnerability presence, impact, and mitigation were made by the student team.

## Tools Used

- OpenAI Codex / ChatGPT: assisting with assignment guidance and conducting mock Q&A sessions.
- JADX: used manually for APK decompilation and inspection of Java code.
- Android Emulator: used manually to demonstrate application behaviour during PoC preparation.

## Documents and References Used

- Android Emulator documentation: https://developer.android.com/studio/run/emulator
- JADX project documentation: https://github.com/skylot/jadx
- Java `Random` documentation: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Random.html
- Java `SecureRandom` documentation: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/security/SecureRandom.html

# Mock Q&A Simulation

**User:** Anonymous (95dcvdtpwm@privaterelay.appleid.com)  
**Created:** 3/29/2026 5:45:04  
**Updated:** 3/29/2026 5:56:37  
**Exported:** 3/29/2026 5:57:04  
**Link:** [https://chatgpt.com/c/69c821a6-25ec-83a1-a2f5-b331d33fbd3e](https://chatgpt.com/c/69c821a6-25ec-83a1-a2f5-b331d33fbd3e)  

## Prompt:
assignment1-rubric-2.pdf

PDF

Cybersecurity\_assignment1 (1).pdf

PDF

this is our report and assgianment rubric, I need you to simulate a mock Q&A for my assignment submission. Based on this rubric, please ask me questions as if you were a tutor evaluating my work. I will answer them, and then I need to record this process in my AI log. Please ask me 3-5 questions covering: - Vulnerability discovery - Threat model - Technical analysis - Mitigation After I answer, please give feedback on what I missed or could improve. Ask one question at a time. At the end, summarise the key questions and improvement actions for inclusion in an AI usage log.

## Response:
Great — we’ll simulate this like a real tutor Q&A based on your rubric

assignment1-rubric-2

. I’ll ask **one question at a time**, and I’ll push you slightly to reach “Exceeds Expectations” level.

* * *

### 🎯 Question 1 (Vulnerability Discovery)

**Can you clearly explain _how_ you discovered the vulnerability, and why this is specifically a _cryptographic/randomness_ issue rather than just a general coding mistake?**

👉 Try to include:

*   The tools + AI role
*   The exact code pattern you identified
*   Why `java.util.Random` is insecure in this context (not just “it’s predictable”)

* * *

Answer this, and I’ll give you feedback before moving to the next question.

## Prompt:
I discovered the vulnerability through static analysis using JADX, where I decompiled the APK and systematically searched for code patterns related to randomness, specifically Random, token, and session, which led me to the generateSessionToken() method in Login.java, revealing that it used java.util.Random to generate authentication tokens. This is specifically a cryptographic/randomness issue rather than a general coding mistake because the security property violated is predictability of secret values: session tokens are cryptographic secrets that must be unpredictable to an adversary, and using a noncryptographic PRNG breaks the fundamental requirement of unpredictability, a general coding mistake would be something like null pointer dereferences or logic errors, whereas here the flaw lies in using an algorithm whose outputs are mathematically deterministic and reversible, directly undermining the secrecy of authentication credentials.

## Response:
Good answer — you’re already close to **“Exceeds Expectations” (5–6 marks)** for this criterion. Let me give you targeted feedback like a tutor would:

* * *

### ✅ What you did well

*   **Clear discovery process**: You mentioned JADX + keyword search (Random, token, session), which aligns well with your methodology
    Cybersecurity\_assignment1 (1)
*   **Correct vulnerability identification**: You clearly linked it to `generateSessionToken()`
*   **Strong conceptual understanding**: Your explanation of _why this is a cryptographic issue_ (unpredictability of secrets) is excellent — this is exactly what tutors look for in Q&A

* * *

### 🔧 What you could improve (to secure full marks)

1.  **Be more concrete about AI assistance**
    *   You said you “searched patterns,” but the rubric explicitly values **AI-assisted workflow explanation**
    *   Improve by saying something like:
        > “I used AI to help identify suspicious API usage patterns like `java.util.Random` in security-sensitive contexts…”
2.  **Tie directly to cryptographic standards / CWE**
    *   You mentioned unpredictability (good), but you could strengthen it by referencing:
        *   **CWE-338 (Use of Cryptographically Weak PRNG)**
    *   This shows _validation_, which the rubric emphasizes
3.  **Explain _how predictability happens_ (technical depth)**
    *   You said “deterministic and reversible” — good, but go one step deeper:
        *   Mention **Linear Congruential Generator (LCG)**
        *   Mention **48-bit seed → state recovery**
    *   This transitions nicely into technical analysis (next question)

👉 Example upgrade sentence:

> “`java.util.Random` is based on a linear congruential generator with a 48-bit internal state, meaning that with enough observed outputs, an attacker can reconstruct the seed and predict future tokens.”

* * *

### 🧠 Summary of improvement for your AI log

*   Add **AI role clarity**
*   Reference **CWE / security standards**
*   Include **technical mechanism of predictability (LCG, seed recovery)**

* * *

### 🎯 Question 2 (Threat Model)

**Your report defines a local attacker — can you justify _why this attacker model is realistic_, and explain what assumptions are critical for the attack to succeed?**

👉 Push yourself to cover:

*   Why “local attacker” is valid in Android context
*   What capabilities matter most (e.g., observing tokens, rooted device)
*   What assumptions, if removed, would break the attack

* * *

Answer this, and I’ll give feedback before moving on.

## Prompt:
The local attacker model is realistic because it reflects common real-world scenarios where multiple users share a device (e.g., family devices, testing devices, or managed enterprise phones) or where an attacker gains local access through a malicious app that can read SharedPreferences on a rooted device, both of which are practical and documented attack vectors in Android security assessments. The critical assumptions for the attack to succeed are: 1. the same Random instance is reused across different user sessions rather than being instantiated per token, enabling state recovery from observed outputs. 2.the attacker can observe a sufficient sequence of their own session tokens to reconstruct the 48-bit PRNG state. 3. the session token is stored and used as the sole authentication credential without additional validation like server-side nonces or time-based expiration.

## Response:
Nice — this is a **strong answer**, and you’re clearly thinking beyond surface-level description. You’re around **high “Meets” → low “Exceeds”** right now. Let’s refine it to full marks.

* * *

### ✅ What you did well

*   **Realism of attacker**: Good justification (shared devices, rooted phones, malicious apps) — very relevant for Android
*   **Critical assumptions**: Excellent — especially:
    *   observing multiple tokens
    *   token as sole authentication credential
*   You’re already thinking like an attacker, not just describing one 👍

* * *

### 🔧 What to improve (to hit full 4/4 marks)

1.  **Clarify one incorrect / risky assumption**
    *   You said:
        > “same Random instance is reused across sessions”
    *   ⚠️ This is **not necessarily required**
    *   Even if a new `Random()` is created each time, it is seeded with `System.nanoTime()` → still predictable

👉 Improve by saying:

> “Even if a new Random instance is created per session, its seed is derived from system time, which has low entropy and can be guessed or narrowed down.”

* * *

2.  **Explicitly link attacker → vulnerability**  
    The rubric wants:

> attacker goals/capabilities explicitly linked to the vulnerability
> 
> assignment1-rubric-2

Right now, you listed capabilities, but didn’t explicitly connect them:

👉 Add a sentence like:

> “These capabilities allow the attacker to collect PRNG outputs, which directly enables state inference and prediction of future session tokens.”

* * *

3.  **Mention weakest/strongest assumption (very high-level insight)**  
    To impress tutors, briefly evaluate:

👉 Example:

> “The most critical assumption is the ability to observe multiple tokens; without this, state recovery becomes significantly harder, reducing exploitability.”

* * *

### 🧠 Summary for your AI log

*   Correct assumption: token observation + weak PRNG
*   Improve:
    *   Don’t rely on “same Random instance”
    *   Explicitly link attacker capability → exploit
    *   Evaluate which assumptions are critical

* * *

### 🎯 Question 3 (Technical Analysis)

Now I’ll push you deeper — this is where many students lose marks.

**Can you explain _technically_ how an attacker could recover or predict future outputs of `java.util.Random` from observed tokens?**

👉 Try to include:

*   What algorithm `Random` uses
*   What information each token leaks
*   How state recovery or brute-force works in practice

* * *

Answer this — this is the most important question for top marks.

## Prompt:
java.util.Random uses a linear congruential generator (LCG) with a 48-bit internal state, where each call to nextInt() exposes 32 bits of the state, and each token character generated via nextInt(62) leaks approximately 6 bits through observable constraints—because the LCG is deterministic and reversible, an attacker who collects enough consecutive outputs from a shared Random instance can either mathematically recover the full 48-bit state using lattice reduction or brute-force it within days, after which all future outputs—and thus other users' session tokens—become fully predictable.

## Response:
This is a **very strong answer** — you’re clearly operating at a high technical level. You’re already in **“Exceeds Expectations” territory**, but I’ll fine-tune it to make it _exam-perfect_.

* * *

### ✅ What you did well

*   **Correct algorithm**: You identified LCG + 48-bit state ✔️
*   **Information leakage reasoning**: ~6 bits per character → excellent ✔️
*   **State recovery idea**: Mentioning lattice reduction + brute force shows depth ✔️
*   **Clear link to prediction**: You connected recovery → future token prediction ✔️

This is exactly what tutors want for **technical analysis marks**

assignment1-rubric-2

* * *

### 🔧 What to improve (to reach _top-tier clarity_)

1.  **Clarify one technical inaccuracy (important)**  
    You said:

> “each call to nextInt() exposes 32 bits of the state”

⚠️ More precise explanation:

*   `nextInt(62)` **does NOT expose full 32 bits**
*   It leaks **partial information** (bounded output)

👉 Improve to:

> “each `nextInt(62)` call leaks partial information about the internal state, since the output is reduced modulo 62”

* * *

2.  **Make the attack more _procedural_ (step-by-step)**  
    Right now it’s conceptually strong, but tutors love **clear attack flow**:

👉 Add structure like:

1.  Collect tokens
2.  Convert characters → numeric outputs (0–61)
3.  Infer constraints on internal state
4.  Use brute-force or constraint solving to recover seed
5.  Predict future outputs

* * *

3.  **Tie back to _practical feasibility_**  
    You mentioned “within days” — good, but strengthen it:

👉 Add:

> “Because the state space is only 2^48, modern hardware can feasibly brute-force or narrow the seed space, especially when combined with observed outputs.”

* * *

4.  **Link explicitly to security impact**  
    Make the final sentence sharper:

👉 Instead of:

> “tokens become predictable”

Say:

> “This allows attackers to generate valid future session tokens and bypass authentication.”

* * *

### 🧠 Summary for your AI log

*   Strong: LCG, entropy leakage, state recovery
*   Improve:
    *   Be precise about `nextInt(62)` leakage
    *   Add **step-by-step attack process**
    *   Emphasize **practical feasibility (2^48)**
    *   Explicitly link to **authentication bypass**

* * *

### 🎯 Question 4 (Mitigation)

Final question — often underestimated, but easy marks if done well.

**Why does replacing `java.util.Random` with `SecureRandom` actually fix the vulnerability, and are there any additional improvements you would recommend beyond this change?**

👉 Push for:

*   _Why_ SecureRandom is secure (not just “it’s better”)
*   Any extra improvements (token length, storage, validation, etc.)

* * *

Answer this, then I’ll give final feedback + a full AI log summary for you to use directly.

## Prompt:
Replacing java.util.Random with SecureRandom fixes the vulnerability because SecureRandom uses a CSPRNG that draws entropy from the operating system's entropy pool and employs a nonlinear, backtracking resistant algorithm meaning its internal state cannot be feasibly recovered from observed outputs, and past outputs cannot be reconstructed even if the current state is compromised, which eliminates the predictability that enabled session token hijacking. Beyond this change, I would recommend increasing token length to at least 32 characters (192+ bits) to ensure sufficient entropy even if the CSPRNG temporarily weakens

## Response:
Excellent — this is a **strong final answer**, and overall your performance across all questions is close to **full marks (14–15/15 level)**. I’ll give final feedback and then a clean **AI log summary you can directly paste**.

* * *

✅ Final Feedback on Mitigation
------------------------------

### What you did very well

*   **Correct reasoning about SecureRandom**
    *   You mentioned:
        *   OS entropy pool ✔️
        *   CSPRNG ✔️
        *   unpredictability & non-recoverable state ✔️
*   **Security property clearly explained**
    *   You explicitly linked it to _removing predictability_ → excellent
*   **Extra improvement (token length)**
    *   This shows deeper thinking beyond minimal fix — tutors like this

* * *

### 🔧 What you could improve (to guarantee full marks)

1.  **Be slightly more concrete (implementation-level)**
    *   You explained _why_, but adding _how it changes the code behavior_ strengthens it:

👉 Add:

> “SecureRandom generates values using cryptographic algorithms such as SHA1PRNG or platform-native providers, ensuring outputs are not linearly related like LCG.”

* * *

2.  **Add one more _practical mitigation_**  
    To stand out, include at least one of these:
    *   **Token expiration / rotation**
*   **Server-side validation (not only client-side)**
*   **Avoid storing tokens in SharedPreferences (or encrypt them)**

👉 Example:

> “Additionally, session tokens should have expiration and not be the sole authentication factor, reducing the impact even if a token is compromised.”

* * *

3.  **Explicitly connect to “why fix works” in one sentence**  
    Make it very examiner-friendly:

👉 Add:

> “This fix works because it replaces a predictable PRNG with one that is computationally infeasible to predict, eliminating the root cause of the vulnerability.”

* * *

🧠 FINAL AI LOG SUMMARY (you can copy this)
-------------------------------------------

### Key Questions Asked by Tutor

1.  **Vulnerability Discovery**
    *   How was the vulnerability identified using tools and AI?
    *   Why is this specifically a cryptographic/randomness issue?
2.  **Threat Model**
    *   Why is the attacker model realistic?
    *   What assumptions are required for the attack to succeed?
3.  **Technical Analysis**
    *   How can an attacker recover or predict outputs of `java.util.Random`?
    *   What information leakage enables this attack?
4.  **Mitigation**
    *   Why does `SecureRandom` fix the issue?
    *   What additional security improvements are possible?

* * *

### Key Improvements Identified

#### 1\. Vulnerability Discovery

*   Clarify **AI’s role** in identifying insecure patterns
*   Reference **CWE-338 (weak PRNG)**
*   Explain **LCG predictability (48-bit state, deterministic)**

* * *

#### 2\. Threat Model

*   Avoid incorrect assumption (same Random instance not required)
*   Explicitly link:
    *   attacker capability → vulnerability exploitation
*   Identify **critical assumptions** (e.g., token observation)

* * *

#### 3\. Technical Analysis

*   Clarify that `nextInt(62)` leaks **partial state information**, not full 32 bits
*   Present attack as **step-by-step process**:
    1.  Collect tokens
    2.  Map to numeric outputs
    3.  Infer constraints
    4.  Recover seed (brute force / inference)
    5.  Predict future tokens
*   Emphasize **feasibility (2^48 state space)**

* * *

#### 4\. Mitigation

*   Explain **why SecureRandom is secure (CSPRNG, entropy source)**
*   Add **implementation detail** (nonlinear, non-deterministic output)
*   Include additional protections:
    *   longer tokens
    *   token expiration
    *   avoid relying solely on token authentication

* * *

### 🎯 Final Tutor Assessment (Simulated)

*   **System & Threat Model:** 4/4
*   **Vulnerability Discovery:** 5.5–6/6
*   **Technical Analysis:** 5.5–6/6
*   **Mitigation:** 2/2
*   **Q&A Performance:** Strong, clear, technically justified

👉 **Estimated total: 14.5–15 / 15**

* * *

If you want, I can now simulate **a harder follow-up (challenge questions tutors might use to test you under pressure)** — that’s usually what separates top students.



---
Powered by [ChatGPT Exporter](https://www.chatgptexporter.com)
