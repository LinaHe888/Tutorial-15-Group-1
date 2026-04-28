# Candidate Findings Tracker

Use this file to record and compare candidate findings before selecting the final one.

| ID | Target | In Scope? | Vulnerability | Severity | Evidence Status | Keep / Drop | Notes |
|---|---|---|---|---|---|---|---|
| C1 | GitHub Bug Bounty — owned GitHub accounts/repos/orgs | Pending active setup; program explicitly allowed by assignment | Access-control / OAuth / API / Actions permission-boundary candidate | TBD | Scope notes and active-testing playbook created; no active testing yet | Keep | Start with two owned accounts + owned org/repo. Priority: GitHub App/API scope boundary, org/repo role boundary, Actions token/artifact boundary. Many easy items are ineligible. |
| C2 | OpenAI Bugcrowd program | Program allowed by assignment; need logged-in Bugcrowd scope details | Web/API auth or data-exposure candidate | TBD | Public announcement checked; full Bugcrowd scope still needs capture | Maybe | Requires Bugcrowd policy page/scope screenshot before testing. |
| C3 | Other HackerOne/Bugcrowd/Intigriti public program | Not chosen yet | TBD | TBD | Need program selection | Maybe | Choose only if scope is clearer/easier than GitHub. |

## Selection Rule
Only one finding should survive into the final presentation. The final choice should be the strongest valid finding supported by the clearest evidence and the best impact argument.
