# INFO5995 Assignment 1 - Activity Log
## Contribution Matrix (N×N Format)

**Team Members:** Jiakai Bao, Clarissa Mo, Yi Qiao, Jiazhou Shen, Lina He, Handong Ding

---


## N×N Contribution Matrix

### Member Jiakai Bao's Assessment

| Member | Contribution Description |
|--------|------------------------|
| **Jiakai Bao** | Task1: Decompiling APK and using AI for preliminary vulnerability analysis Video editing. |
| **Clarissa Mo** | Task 2: Application functionality, system architecture, and threat model. |
| **Yi Qiao** | Task 3: Detect and analyse vulnerabilities associated with insecure randomness or misuse of cryptographic mechanisms. |
| **Jiazhou Shen** | Task 4: Describe the root causes of these vulnerabilities and outline potential attack vectors that could be leveraged by adversaries. |
| **Lina He** | Task 5: Propose appropriate mitigation strategies and thoroughly document the identified issues along with the required deliverables. |
| **Handong Ding** | Prepare a comprehensive README file and examine all relevant supporting materials. |

### Member Clarissa Mo's Assessment

| Member | Contribution Description |
|--------|------------------------|
| **Jiakai Bao** | |
| **Clarissa Mo** | |
| **Yi Qiao** | |
| **Jiazhou Shen** | |
| **Lina He** | |
| **Handong Ding** | |

### Member Yi Qiao's Assessment

| Member | Contribution Description |
|--------|------------------------|
| **Jiakai Bao** | Task 1 APK decompilation (JADX + APKTool setup and execution), reverse engineering of a1_case1.apk, initial vulnerability identification in Login.generateSessionToken(), team handoff documentation with verified findings and process playbook, APK analysis ZIP packaging |
| **Clarissa Mo** | |
| **Yi Qiao** | Task 3 primary vulnerability discovery (CWE-338: java.util.Random for session token), complete vulnerability chain analysis with 4-step evidence chain (role definition, timing, storage, weak RNG), quantitative attack feasibility analysis (48-bit LCG, 2^48 seed space, days-to-crack timeline), report.pdf contributions (Task 3 section, System and Threat Model), repo branch management and merge coordination |
| **Jiazhou Shen** | Task 4 threat model and exploitation analysis — threat model, root cause analysis, attacker model (rooted-device/reverse engineer), attacker capabilities and preconditions, 6-step attack path, impact and risk analysis (Medium risk) |
| **Lina He** | |
| **Handong Ding** | |

### Member Jiazhou Shen's Assessment

| Member | Contribution Description |
|--------|------------------------|
| **Jiakai Bao** | Task 1: Worked on decompiling the APK, doing the initial analysis, and editing the presentation video. |
| **Clarissa Mo** | Task 2: Worked on analysing the app’s functionality and building the system model. |
| **Yi Qiao** | Task 3: Worked on identifying the vulnerability related to randomness or cryptographic misuse. |
| **Jiazhou Shen** | Task 4: Refined the attacker threat model and explained why the vulnerability exists and how an attacker could potentially exploit it. |
| **Lina He** | Task 5: Worked on mitigation, documenting the findings, and preparing the submission materials. |
| **Handong Ding** | Prepared the comprehensive README and helped review and consolidate the relevant supporting materials for the final submission. |

### Member Lina He's Assessment

| Member | Contribution Description |
|--------|------------------------|
| **Jiakai Bao** |Task1 Decompile the APK and initial analyse and presentation viduo editing. |
| **Clarissa Mo** |Task2 app’s functionality and build system and threat model.|
| **Yi Qiao** |Task3 Identify vulnerability related to randomness or cryptographic misuse.|
| **Jiazhou Shen** |Task4 Explain why the vulnerability exists and how an attacker could exploit it.|
| **Lina He** |Task5 Mitigation and document the findings clearly and required submission materials.|
| **Handong Ding** |writing the README and checking and reviewing materials. |

### Member Handong Ding's Assessment

| Member | Contribution Description |
|--------|------------------------|
| **Jiakai Bao** | Responsible for decompiling the APK using JADX to reconstruct the Java source code. Collaborated with the team to perform preliminary vulnerability scans and managed the final video production. |
| **Clarissa Mo** | Defined the application’s functional architecture and mapped the data flows between MainActivity, Login, and Profile. Contributed to identifying the protected assets and established the initial threat boundaries. |
| **Yi Qiao** | Focused on detecting cryptographic weaknesses, specifically locating the 48-bit PRNG flaw in Login.java and the plaintext storage issue in MainActivity. Collaborated on analyzing the technical root causes. |
| **Jiazhou Shen** | Developed the attack vectors and refined the PoC (Proof of Concept) logic to demonstrate how PRNG seed reconstruction leads to session hijacking. Worked with the team to verify the exploitability of identified flaws. |
| **Lina He** | Proposed cryptographic mitigation strategies, including the implementation of SecureRandom and increasing token entropy. Responsible for thorough documentation of all identified issues and required deliverables. |
| **Handong Ding** | Conducted independent feasibility validation for Tasks 1-5 during the project startup and established the preliminary threat & security models and reporting framework & draft. Collaborated with the team to discuss and refine the logic for the PoC verification methods. |

---

## Team Agreement

We confirm that all members contributed roughly equally to this assignment. Each member participated in analysis, documentation, and presentation preparation.
