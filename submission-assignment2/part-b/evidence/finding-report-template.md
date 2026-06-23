# Finding Report Template

Use this once there is a plausible candidate. Keep all values factual and evidence-backed.

## Title

`<Impact> via <root cause> in <feature>`

Example:

`Cross-resource data exposure via indirect prompt injection in AI document summarization`

## Target

- Program:
- Platform:
- Target URL:
- Asset:
- Scope status:
- Scope evidence path:

## Finding Type

- Confirmed non-zero-day:
- Zero-day candidate:
- Reasoning:

## Preconditions

- Required feature:
- Required account type:
- Required permissions:
- Required victim/user action:
- Required attacker control:

## Test Accounts

- Account A:
- Account B:
- Workspace/repo/project:
- Synthetic canary:

## Steps to Reproduce

1. Create test resource:
2. Add attacker-controlled content:
3. Add private/sensitive test resource:
4. Ask the AI feature:
5. Observe output/action:
6. Repeat:

## Expected Result

The AI feature should follow the user's instruction, treat retrieved content as untrusted, and enforce authorization boundaries before using or revealing data.

## Actual Result

Describe only what happened. Include transcript snippets and evidence file paths.

## Reproducibility

- Attempts:
- Successful attempts:
- Repro rate:
- Conditions that affect reliability:

## Impact

Explain the concrete security consequence:

- data exposed:
- unauthorized action:
- affected users/workspaces:
- attacker effort:
- default settings or special configuration:

Avoid overclaiming. If the evidence only proves synthetic data exposure in test accounts, say that clearly and explain why the same boundary exists for real data.

## Severity Mapping

- Platform severity:
- CVSS vector if needed:
- Normalized tier:
- Justification:

## Novelty Evidence

Search and save:

- program disclosed reports
- public advisories
- vendor docs
- general web search
- GitHub issues if relevant

Record why this exact chain appears new.

## Recommended Fix

Suggested mitigations:

- isolate trusted user instructions from untrusted retrieved content
- apply authorization checks before retrieval and again before generation
- prevent untrusted content from triggering tool calls
- require explicit user confirmation for sensitive actions
- sanitize or isolate AI-rendered markup
- log and alert on prompt-injection attempts that request data movement or tool abuse

## Evidence Index

| Evidence | Path | Purpose |
|---|---|---|
| Scope screenshot | `scope/...` | Confirms target is in scope |
| Input content | `evidence/...` | Shows attacker-controlled source |
| Transcript | `evidence/...` | Shows AI behavior |
| Repro summary | `evidence/...` | Shows reliability |
| Screenshots/video | `evidence/...` | Supports impact |

