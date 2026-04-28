# Part B Hunting Plan — 2026-04-28

Branch: `joey-a2-partb`

## Goal

Find 3-5 legal candidate findings, then present exactly one highest-impact valid in-the-wild finding.

## Current recommended starting target

Primary candidate: **GitHub Bug Bounty**

Reason: It is explicitly allowed by the assignment, has clear public scope/rules, and supports safe testing using only owned accounts/repos/orgs.

## Search workflow

### Step 1 — Pick target from recommended program list

Use only:
- HackerOne / Bugcrowd / Intigriti / YesWeHack / Immunefi public programs; or
- explicitly allowed company-run programs from the assignment spec: Google, Meta, Microsoft, Apple, GitHub, OpenClaw, Anthropic, OpenAI.

Reject a target if:
- scope is unclear;
- target is company-run but not explicitly listed and has no coordinator approval;
- testing requires touching real users' data;
- policy forbids the vulnerability class we want to test.

### Step 2 — Record scope evidence before testing

For every candidate, save:
- target URL/program URL;
- exact in-scope asset/domain;
- prohibited testing list;
- severity policy/mapping;
- why our planned test is non-disruptive.

### Step 3 — Hunt only low-risk classes first

Safe first-pass classes:
- access-control/IDOR using two owned accounts only;
- OAuth callback / redirect_uri edge cases in owned test apps only;
- CSRF on low-risk actions in owned accounts only;
- XSS with harmless payloads in owned content only;
- CORS misconfiguration checks using simple browser/request observations, no credential theft;
- information disclosure limited to our own data.

Avoid:
- DoS/resource exhaustion;
- mass scanning/bruteforce;
- testing other users' repos/orgs/data;
- uploading real malware or destructive payloads;
- phishing/social engineering.

### Step 4 — Candidate validation checklist

A candidate is worth keeping only if we can show:
- in-scope target;
- exact reproducible steps;
- root cause or security control failure;
- real impact, not just theoretical behavior;
- screenshots/logs/redacted proof artifacts;
- novelty search: public reports/CVEs/docs checked;
- severity mapping to program label or CVSS fallback.

### Step 5 — Final selection rule

Choose the single candidate with the best combination of:
1. validity/reproducibility;
2. highest demonstrated impact;
3. novelty evidence;
4. clean/safe proof artifacts;
5. ability to explain in 5 minutes and defend in Q&A.

## GitHub-specific first pass

Required setup before active testing:
- two owned GitHub accounts, ideally marked as bounty research accounts if submitting externally;
- one owned test repository;
- one owned test organization;
- optional owned GitHub App/OAuth App for permission testing.

Initial test ideas:
1. Permission boundary checks between owner/member/outside collaborator in an owned repo/org.
2. GitHub App/OAuth App scope confusion: verify whether an app/token can access resources outside granted scopes.
3. Private/public state transition edge cases: ensure private repo metadata, issue titles, discussions, packages, Actions artifacts remain inaccessible to Account B.
4. Low-risk CSRF/redirect checks on owned account flows only.

Do not test: other people's private repos, large automated scans, account enumeration, GitHub Pages takeover, user-hosted Pages XSS, or any ineligible list item.
