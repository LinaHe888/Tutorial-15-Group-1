# GitHub Bug Bounty Scope Notes

Source pages checked on 2026-04-28:
- https://bounty.github.com/
- https://bounty.github.com/scope
- https://bounty.github.com/rules
- https://bounty.github.com/targets
- https://bounty.github.com/ineligible
- https://bounty.github.com/rewards
- https://bounty.github.com/targets/github-api
- https://bounty.github.com/targets/github-pages

## Why this target is allowed

GitHub Bug Bounty is explicitly allowed in the Assignment 2 spec as a company-run program.
It has a public HackerOne submission path and legal safe-harbor language when testing follows the program rules.

## In-scope domains/assets noted

From GitHub scope page:
- `github.com` and most subdomains, except explicitly excluded subdomains.
- `githubassets.com` and subdomains.
- `githubusercontent.com` and subdomains.
- `githubapp.com` and subdomains, except listed exclusions.
- `githubwebhooks.net` and subdomains.
- `github.net` and subdomains.
- `npmjs.com` and subdomains.
- `npmjs.org` and subdomains.
- GitHub CLI, Desktop, Mobile, Enterprise Server, Enterprise Cloud.

## Useful focus areas

From GitHub target pages:
- GitHub API: privilege escalation via GitHub Apps/OAuth Apps; access to private resources without correct token scope; SAML/organization access-policy bypasses; GraphQL resource limitation bypasses.
- GitHub Pages: build-process issues such as arbitrary code execution or arbitrary file read during builds. Note: user-hosted Pages content vulnerabilities and Pages subdomain takeover are listed as ineligible.

## Hard safety boundaries

From GitHub rules/ineligible pages:
- Do not test repositories, organizations, or user data not owned by the tester.
- For authorization-bypass testing, use only accounts owned by the team.
- No social engineering, phishing, physical attacks, spam, DDoS, or excessive automated scanning.
- Do not intentionally access others' PII; if accidental exposure occurs, stop and report with redaction.
- Several common low-value ideas are ineligible, including many GitHub Pages hosted-content issues, subdomain takeover, email/username enumeration, simple host-header injection, public OAuth client secrets in official clients, and vulnerabilities in third-party repos/packages.

## Assignment fit

Pros:
- Clearly in the assignment's allowed list.
- Strong public scope/rules pages for evidence.
- Severity examples map cleanly to Assignment 2 S1-S4 tiers.
- Can test safely using owned GitHub accounts/repos/orgs.

Cons:
- Very mature target; true zero-day is hard.
- Many easy findings are explicitly ineligible.

Recommended use: primary structured hunting target if the team can create two owned GitHub accounts and one test organization/repository. Otherwise keep as a scope/evidence example and shortlist OpenAI/Bugcrowd programs too.
