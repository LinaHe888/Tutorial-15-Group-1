# GitHub Bug Bounty Active Testing Playbook

Status: planning only. Do not run tests until the team has owned test accounts/assets.

## Core principle

Every validation step must use only assets controlled by the team:
- Account A: owner/admin
- Account B: lower-privilege user, member, collaborator, or outsider
- Owned test organization
- Owned test repositories/packages/projects
- Optional owned GitHub App/OAuth App

Stop immediately if any step returns data that belongs to a real third party.

## Best hunting surfaces from GitHub's own focus areas

### Surface 1 — Organization/repository permission boundaries

Why it fits:
- GitHub's target page explicitly calls out escalation from repo admin to org admin, bypassing repository permissions in orgs, branch protection bypasses, and collaboration features.
- It can be tested with owned org/repo/accounts.

Test matrix:
| Scenario | Setup | Expected secure behavior | Evidence to capture |
|---|---|---|---|
| Outside collaborator tries org-only action | A owns org/repo; B invited to one repo only | B cannot view/change org-level settings, members, teams, billing/security settings | screenshots/API responses with 403/404 redacted |
| Write/maintain role tries admin-only repo action | B has write/maintain, not admin | B cannot make private repo public, change branch protection, change secrets, install apps with elevated perms | UI/API evidence |
| Team/project/discussion access | private repo/org project/team discussion visible to A only | B cannot access titles/content via UI/API/notifications/search | response codes and screenshots |
| Private/public transition | switch owned repo private/public/private | metadata from private state should not leak to unauthorized B | before/after screenshots |

Promising impact if broken:
- Unauthorized read of private repo/project/team data: likely Medium/High depending sensitivity.
- Unauthorized permission change/publication of private repo: High.
- Escalation to org admin: Critical/High.

### Surface 2 — GitHub API and GraphQL permission checks

Why it fits:
- GitHub API focus areas include private resource access without correct token scope, OAuth/GitHub App privilege escalation, team discussions, SAML/organization access-policy bypasses, and GraphQL resource limits.

Safe test approach:
- Create tokens/apps only for owned accounts/orgs.
- Compare UI permission vs REST vs GraphQL behavior for the same resource.
- Use minimal requests; no brute force, no enumeration.

Test ideas:
| Scenario | Setup | Expected secure behavior |
|---|---|---|
| Token without `repo` scope reads private repo metadata | A creates private repo; token has limited scope | API must not reveal private metadata beyond allowed baseline |
| GitHub App installed on repo X tries repo Y | A owns two private repos; app installed only on repo X | app token must not read repo Y |
| OAuth App scope mismatch | OAuth app requests low scope | app must not access org/private resources needing higher scope |
| GraphQL node ID reuse | B knows node ID of private issue/project from A-owned data | GraphQL should return null/permission error, not title/body |

Promising impact if broken:
- Cross-repo/private-resource access: High.
- Limited private metadata like issue title only: Medium.

### Surface 3 — GitHub Actions permission and artifact boundaries

Why it fits:
- GitHub Actions focus areas include repo isolation, repo-scoped `GITHUB_TOKEN`, secrets handling, app/OAuth ability to edit workflow files, and access to resources from other repositories' jobs.

Safe test approach:
- Use owned repos only.
- Avoid resource exhaustion and heavy workflows.
- Do not try to attack GitHub infrastructure; only validate permission boundaries.

Test ideas:
| Scenario | Setup | Expected secure behavior |
|---|---|---|
| `GITHUB_TOKEN` from repo A accesses private repo B | A owns both repos; repo A workflow calls API for repo B | denied unless explicitly granted by normal auth |
| Low-privilege app edits workflow file | App has content permission but not workflows/admin where applicable | should not modify `.github/workflows/*` in a way that escalates to secrets |
| Artifact metadata permission | Token without artifact metadata permission attempts artifact metadata API | denied or limited as documented |
| PR from fork and secrets | Owned fork PR attempts to access secrets | secrets unavailable as documented |

Promising impact if broken:
- Access private content outside repo via Actions token: High.
- Secret exposure across repo/org boundaries: High/Critical.

### Surface 4 — Copilot / AI policy controls (lower priority)

Why lower priority:
- Many prompt-injection-only reports are explicitly ineligible.
- Eligible only if there is a concrete auth/policy bypass, cross-repo/tenant data exposure, or unauthorized action.

Potential route if team has Copilot/Enterprise access:
- Test content exclusion / repository access / agent firewall settings using owned org/repo only.
- Verify excluded private content is not accessible to Copilot features.

Do not submit:
- "Prompt injection changes Copilot's answer" without access-control impact.
- Bad/insecure code suggestions.
- Fake token suggestions.

## Recommended order for Joey's team

1. **Start with GitHub API / GitHub App boundary tests** — highest chance of clear reproducible evidence without dangerous behavior.
2. **Then org/repo permission matrix** — easy to explain in presentation; strong impact if broken.
3. **Then Actions token/artifact boundary tests** — good impact, but be careful not to drift into ineligible resource abuse.
4. **Skip Copilot unless the team already has the needed paid/enterprise access.**

## Evidence template for each attempted test

For each candidate attempt, record:
- Date/time and tester
- Program page and scope URL
- Owned accounts/assets used
- Preconditions/roles/tokens/scopes
- Steps performed
- Expected secure result
- Actual result
- Screenshots/logs/API responses, redacted
- Security impact if actual result is wrong
- Novelty search terms used
- Keep/drop decision

## Stop conditions

Stop and do not continue testing if:
- a request returns other users' data;
- testing would require guessing/enumerating real users/resources;
- a test requires high request volume;
- the issue appears higher than Medium severity: per assignment guidance, sanity-check with tutor/coordinator before broad external submission.
