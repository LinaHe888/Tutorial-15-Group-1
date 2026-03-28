# AI Usage Summary

This file summarises how AI tools were used in this assignment. Summaries are provided instead of full prompt-response transcripts.

## Scope of AI Use

AI was used as a support tool for explanation, organisation, and presentation preparation. It was not used as a substitute for manual reverse engineering, manual evidence collection, or final security judgement.

## Task 1

AI assistance was used to help explain decompiled Android code structure and to turn technical findings into short written summaries. The actual APK inspection, decompilation review, and interpretation of application logic were completed manually.

## Task 2

AI assistance was used to help organise system-model notes into clearer written explanations and to improve wording for security-relevant interactions, trust boundaries, and attack surfaces. Final modelling decisions remained based on manual analysis of the target application and assignment requirements.

## Task 3

AI assistance was used to:

- explain the role of `checkCredentials()`, `createSession()`, `generateSessionToken()`, and `new Random()` in the vulnerability chain;
- help organise the vulnerability evidence into a clear Proof of Concept narrative;
- draft concise English and Chinese explanations for the vulnerability;
- prepare a PoC video script, including a login demonstration on an Android emulator and a code-evidence walkthrough;
- suggest practical recording steps using `adb` and Android Emulator.

The vulnerability itself was manually confirmed through APK decompilation, direct review of the relevant code paths, and manual inspection of screenshots and notes.

## Task 4

AI assistance was used to improve the structure and clarity of threat-model explanations, including attacker goals, assets, abuse paths, and mitigations. The substantive threat analysis and risk conclusions were manually reviewed before inclusion in the submission.

## Task 5

AI assistance was used to:

- draft concise summaries suitable for the report and presentation;
- prepare an AI usage statement;
- refine the wording of PoC explanations and video narration;
- help produce a short summary of how AI was used across the assignment.

## Human Verification and Limits

All AI-generated wording was manually reviewed and edited before use. AI outputs were treated as drafting assistance only. Decompiled code, screenshots, notes, and assignment artefacts were checked manually to confirm that explanations matched the actual application behaviour. Final conclusions about vulnerability presence, impact, and mitigation were made by the student team.

## Tools Used

- OpenAI Codex / ChatGPT: used for explanation, drafting, PoC narration, and workflow suggestions.
- JADX: used manually for APK decompilation and inspection of Java code.
- Android Emulator: used manually to demonstrate application behaviour during PoC preparation.
- Android Debug Bridge (`adb`): used manually for APK installation, app launching, and screen recording.

## Documents and References Used

- Assignment materials in the project repository, including README files, task notes, and submission drafts.
- [AI Usage Log.md](/Users/lina/Desktop/Tutorial-15-Group-1-main 2/submission/task-5-report-and-presentation/ai-log/AI Usage Log.md)
- [task3-video-script.md](/Users/lina/Desktop/Tutorial-15-Group-1-main 2/submission/task-3-vulnerability-discovery/task3-video-script.md)
- [official-links.md](/Users/lina/Desktop/Tutorial-15-Group-1-main 2/references/official-links.md)
- Android Debug Bridge documentation: https://developer.android.com/tools/adb
- Android Emulator documentation: https://developer.android.com/studio/run/emulator
- JADX project documentation: https://github.com/skylot/jadx
- Java `Random` documentation: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Random.html
- Java `SecureRandom` documentation: https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/security/SecureRandom.html

## Short Submission-Friendly Summary

AI was used primarily to improve clarity, structure, and presentation. It helped explain decompiled code, organise the vulnerability evidence chain, draft PoC narration, and summarise work across tasks. AI was not relied on as the sole source of security findings. All technical conclusions were manually verified against the APK, decompiled code, screenshots, and assignment materials.
