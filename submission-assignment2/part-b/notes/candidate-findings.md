# Candidate Findings Tracker

Use this file to record and compare candidate findings before selecting the final one.

| ID | Target | In Scope? | Vulnerability | Severity | Evidence Status | Keep / Drop | Notes |
|---|---|---|---|---|---|---|---|
| C1 | GitHub Bug Bounty — owned GitHub accounts/repos/orgs | Pending active setup; program explicitly allowed by assignment | Access-control / OAuth / API / Actions permission-boundary candidate | TBD | Scope notes and active-testing playbook created; no active testing yet | Keep | Start with two owned accounts + owned org/repo. Priority: GitHub App/API scope boundary, org/repo role boundary, Actions token/artifact boundary. Many easy items are ineligible. |
| C2 | Microsoft Azure DevOps / Microsoft Identity | Program allowed by assignment; exact product scope must be captured before testing | Identity/OAuth/tenant or Azure DevOps permission boundary | TBD | Passive shortlist created; needs owned tenant setup | Maybe | Good backup if team has Microsoft/Azure test tenant. Keep all testing inside owned accounts/tenants. |
| C3 | OpenAI Bugcrowd program | Program allowed by assignment; need logged-in Bugcrowd scope details | Web/API auth or data-exposure candidate | TBD | Public policy checked; full Bugcrowd scope still needs capture | Maybe | Requires Bugcrowd policy page/scope screenshot before testing. |
| C4 | Google VRP | Program allowed by assignment; rules are dynamic/sign-in heavy | Google account/cloud/OAuth authorization boundary | TBD | Passive shortlist only | Backup | Use only if scope can be captured clearly and testing stays inside owned accounts/projects. |
| C5 | Anthropic Model Safety Bug Bounty | Allowed but requires application/HackerOne invite/NDA for model-safety testing | Universal jailbreak / model-safety issue, not ordinary app vuln | TBD | Public program page checked | Drop unless accepted | Not recommended for ordinary Part B because evidence/presentation may be constrained. |

## Selection Rule
Only one finding should survive into the final presentation. The final choice should be the strongest valid finding supported by the clearest evidence and the best impact argument.
