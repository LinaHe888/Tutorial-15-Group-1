# Part B Test Matrix

Use one row per test. Keep all canaries synthetic and unique.

| ID | Date | Target | Program URL | Scope proof saved? | Test accounts | Canary | Vector | Expected | Actual | Repro rate | Severity guess | Evidence path | Next action |
|---|---|---|---|---|---|---|---|---|---|---:|---|---|---|
| B-001 | 2026-04-29 | TBD | TBD | No | TBD | `INFO5995_CANARY_20260429_001` | TBD | TBD | TBD | 0/0 | TBD | TBD | Select target |

## Evidence File Naming

Recommended names:

- `scope/<target>-scope-YYYYMMDD.png`
- `evidence/<test-id>-input.md`
- `evidence/<test-id>-transcript.md`
- `evidence/<test-id>-audit-log.png`
- `evidence/<test-id>-network-log.txt`
- `evidence/<test-id>-repro-summary.md`

## Stop Conditions

Stop testing and document the event if:

- real user data appears
- an action would affect a third party
- the program scope forbids the technique
- the product asks for confirmation before a sensitive action and the requested action is not harmless
- the only way to continue would require spam, phishing, DoS, credential capture, or destructive changes

