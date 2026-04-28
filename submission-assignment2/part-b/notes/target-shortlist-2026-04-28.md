# Part B Target Shortlist — 2026-04-28

Method: passive research only. No target testing/scanning performed.

## Recommendation summary

| Rank | Target | Program URL | Beginner fit | Why |
|---|---|---|---|---|
| 1 | GitHub Bug Bounty | https://bounty.github.com/ | Best | Explicitly allowed by assignment; scope/rules are public and detailed; owned-account testing is practical. |
| 2 | Microsoft Azure DevOps / Microsoft Identity | https://www.microsoft.com/en-us/msrc/bounty-programs | Good but broader | Explicitly allowed; strong safe-harbor/rules. Use only if team already has Microsoft/Azure test accounts and can keep all tests inside owned tenants. |
| 3 | OpenAI Bugcrowd | https://bugcrowd.com/openai | Maybe | Explicitly allowed and current policy points to Bugcrowd. Full scope often needs Bugcrowd login; capture scope before any testing. |
| 4 | Google VRP | https://bughunters.google.com/about/rules | Maybe/Hard | Explicitly allowed, but public pages are dynamic/sign-in heavy; very mature target. Only use if scope can be captured clearly. |
| 5 | Anthropic Model Safety Bug Bounty | https://support.anthropic.com/en/articles/12119250-model-safety-bug-bounty-program | Not recommended for this assignment unless accepted | Allowed, but primarily model-safety jailbreak work via HackerOne application/NDA; less aligned with ordinary web security Part B evidence. |

## 1. GitHub Bug Bounty — Primary target

Program URL:
- https://bounty.github.com/

Scope clarity:
- Very clear. Public pages for scope, targets, rules, ineligible submissions, rewards.
- In-scope includes `github.com`, `githubassets.com`, `githubusercontent.com`, `githubapp.com`, `githubwebhooks.net`, `github.net`, npm domains, and official GitHub clients/products, with listed exceptions.

Safe owned-account classes:
- Authorization / role-boundary bypass in owned org/repo.
- GitHub App/OAuth App scope mismatch using owned apps and owned repos.
- REST vs GraphQL permission inconsistencies on owned private resources.
- GitHub Actions repo boundary, token boundary, artifact metadata boundary using owned repos only.

Major pitfalls:
- Many easy ideas are explicitly ineligible: user-hosted Pages issues, Pages subdomain takeover, email/username enumeration, simple host-header injection, public OAuth secrets in official clients, vulnerabilities in third-party repos/packages.
- Do not touch other users' repositories, orgs, data, or PII.
- Avoid high-volume scanning and resource-exhaustion tests.

Recommendation:
- Keep as C1 and start here.

## 2. Microsoft bounty programs — Backup target if Microsoft test tenant is available

Program URLs:
- https://www.microsoft.com/en-us/msrc/bounty
- https://www.microsoft.com/en-us/msrc/bounty-programs
- https://www.microsoft.com/en-us/msrc/pentest-rules-of-engagement
- https://www.microsoft.com/en-us/msrc/bounty-guidelines

Scope clarity:
- Clear top-level rules and program list.
- Separate product pages define exact product-specific eligibility. Candidate programs include Microsoft Identity, Azure, Azure DevOps Services, M365, Dynamics/Power Platform, Defender, and Copilot.

Safe owned-account classes:
- Identity / OAuth / OpenID Connect misconfiguration or authorization bypass inside owned accounts/tenants.
- Azure DevOps permission boundary tests inside owned organization/project/repository.
- M365/Dynamics/Power Platform role-boundary issues inside owned tenant.

Major pitfalls:
- Scope is broad and product-specific; must capture exact program page before testing.
- Testing must not access/modify/exfiltrate customer data or disrupt services.
- GitHub is explicitly outside Microsoft MSRC rules because GitHub has its own process.

Recommendation:
- Good backup if the team has Azure/Microsoft test tenant access. Otherwise GitHub is simpler.

## 3. OpenAI Bugcrowd — Backup / maybe target

Program URLs:
- https://openai.com/policies/coordinated-vulnerability-disclosure-policy/
- https://bugcrowd.com/openai
- https://help.openai.com/en/articles/6653653-how-to-report-security-vulnerabilities-to-openai

Scope clarity:
- OpenAI public policy points to Bugcrowd for detailed guidelines and includes both security and safety bug bounty paths.
- Public fetch gives limited scope detail; team should log into Bugcrowd and screenshot/capture current scope before testing.

Safe owned-account classes:
- Account/session/auth boundary checks using only owned accounts.
- API/project/resource permission checks using owned API org/project/resources.
- Data isolation checks between owned accounts/workspaces only.
- CORS/redirect issues only where policy permits and with non-destructive proof.

Major pitfalls:
- Do not test model jailbreaks under the normal security program unless the safety program explicitly allows it.
- Do not attempt to access other users' chats, files, API data, or PII.
- Full Bugcrowd exclusions must be checked before testing.

Recommendation:
- Keep as C2, but do not start until Bugcrowd scope is captured.

## 4. Google VRP — Maybe target, likely hard

Program URLs:
- https://bughunters.google.com/about/rules
- https://bughunters.google.com/about/rules/google-friends/google-and-alphabet-vulnerability-reward-program-vrp-rules
- https://bughunters.google.com/about/rules/google-friends/4849867320328192/cloud-vulnerability-reward-program-rules

Scope clarity:
- Program is explicitly allowed by the assignment.
- Public pages may require sign-in/dynamic rendering, so capture visible rules/scope directly in browser before any testing.

Safe owned-account classes:
- Account/resource authorization boundary using only owned Google accounts/projects.
- Google Cloud IAM/resource access boundary using owned test projects only.
- OAuth consent/app scope mismatch using owned app/project only.

Major pitfalls:
- Very mature, high competition.
- Dynamic rules/scope pages can make evidence harder unless captured manually.
- Must avoid testing other users, production data, spam, social engineering, and disruptive testing.

Recommendation:
- Keep as C3 only if GitHub/OpenAI/Microsoft do not work and the team can clearly capture scope evidence.

## 5. Anthropic Model Safety Bug Bounty — Not recommended for ordinary Part B

Program URL:
- https://support.anthropic.com/en/articles/12119250-model-safety-bug-bounty-program

Scope clarity:
- Clear but specialized: primarily universal jailbreaks against Constitutional Classifiers through a HackerOne program after application/acceptance.
- Requires application, HackerOne invite, and NDA/confidentiality obligations.

Safe classes:
- Only authorized model-safety tests after acceptance.
- Technical vulnerabilities should go through Anthropic responsible disclosure, not the model-safety bounty.

Major pitfalls:
- Not a normal web/app security target.
- Confidentiality restrictions make class presentation evidence tricky.
- Prompt-only jailbreaks may not map cleanly to Assignment 2's ordinary vulnerability evidence unless program acceptance and rules are clear.

Recommendation:
- Do not use for this assignment unless the team is already accepted and has permission to present sanitized evidence.

## Final working plan

1. Continue with **GitHub C1**.
2. Prepare owned test assets: two accounts, one org, two private repos, optional GitHub App/OAuth App.
3. Run only low-volume permission-boundary checks from `github-active-testing-playbook.md`.
4. If no candidate after one focused session, switch to **Microsoft Azure DevOps/Identity** if owned tenant exists, otherwise **OpenAI Bugcrowd** after capturing full scope.
