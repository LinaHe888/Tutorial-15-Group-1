# HackerOne Variant Hunting — 2026-04-28

## Scope status

Target: HackerOne own bug bounty program (`https://hackerone.com/security`).

Confirmed in-scope assets include:
- `hackerone.com`
- `hackerone.com/graphql`
- `api.hackerone.com`

Testing constraint: low-frequency, read-only where possible, non-destructive, no DoS, no third-party private data.

## Historical reports guiding variants

| Report | Severity | Pattern |
|---|---:|---|
| #489146 | Critical | GraphQL `nodes` path bypassed attribute-level authorization. |
| #792927 | High | GraphQL invitation type exposed user email by username. |
| #807448 | High | Private-program invitation/token/email exposure before acceptance. |
| #1618347 | Critical | GraphQL GID `node(id:)` disclosed private program policy asset groups. |
| #2487889 | Critical | `/bugs.json` accepted org/query filters without sufficient access checks. |
| #2633771 | Medium | `AddTagToAssets` mutation processed foreign tag IDs / custom tag disclosure. |
| #2965723 | Medium | Low-permission API key accessed unauthorized program policy/updates. |
| #3000510 | Critical | `/reports/:id.json` over-serialized sensitive reporter account fields. |

## Work done so far

Evidence files created:
- `evidence/hackerone-security-public-reports-2026-04-28.md`
- `evidence/hackerone-historical-report-excerpts-2026-04-28.txt`
- `evidence/hackerone-initial-probes-2026-04-28.txt`
- `evidence/hackerone-reports-json-current-behavior-2026-04-28.txt`
- `evidence/hackerone-reports-json-keysets-2026-04-28.txt`

Initial observations:
- GraphQL introspection is disabled.
- Basic unauthenticated GraphQL public queries return intended public data only.
- Sensitive fields tested so far (`private_profile`, `team_memberships`, `members`, `invitations`, etc.) are denied or null.
- Common open-redirect quick checks did not hit.
- `/reports/:id.json` still returns rich JSON for public disclosed reports, but current samples appear to be public report representations. Need compare for unexpected sensitive fields only on public disclosed reports or owned/sandbox flows.
- `api.hackerone.com/v1/hackers/programs/...` unauthenticated baseline returns 401 for program, updates, and structured scope endpoints.
- Historical `PolicyPageAssetGroup` GraphQL GID from #1618347 now returns `NOT_FOUND` / undefined old document type, suggesting that exact path is fixed.
- Public report GraphQL `node(id:)` for #3000510 returns reporter email as `null`; no obvious public-report PII leak observed in that path.
- Public structured-scope `node(id:)` attachment checks for first HackerOne scopes returned empty attachment arrays.
- 2026-04-29 owned sandbox report `3702107` created safely for authz testing; authenticated admin positive controls worked, while unauthenticated `/reports/:id(.json)`, GraphQL `report(id:)`, `reports(database_id:)`, `node(id:)`, `/bugs.json`, duplicate modal query, and collaborator query all denied/returned empty as expected.
- 2026-04-29 owned organization GID `node(id:)` checks for `Organization/75849`, `Engagements::BugBountyProgram/112321`, `OrganizationMemberGroup/355011`, `OrganizationMember/1031690`, and `OrganizationInbox/108399` returned expected data as admin and `NOT_FOUND` unauthenticated.
- 2026-04-29 created temporary organization API tokens `info5995-ro-20260429` (read-only group `OrganizationMemberGroup/355011`) and `info5995-nogroup-0429` (no group). Read-only token could access owned program reports via `api.hackerone.com/v1/reports/:id`; no-group token received `403`; GraphQL rejected API-token Basic auth with `Invalid authentication token`; both tokens were revoked and post-revocation checks returned `401`.

## Safe next test hypotheses

1. Compare HTML vs `.json` vs GraphQL for public disclosed reports only; flag unexpected sensitive fields without enumerating private report IDs.
2. If using owned/demo orgs, verify low-permission API keys cannot access sibling program policies/updates.
3. If using owned accounts/assets/tags, test `AddTagToAssets` foreign tag rejection without brute force.
4. If using owned secondary account, test report invitation/collaborator visibility after invite/revoke/leave.
5. Avoid DoS mutation aliasing, private handle probing, sequential ID scanning, or third-party usernames/emails.

## Current candidate quality

No zero-day candidate confirmed yet. HackerOne remains the strongest hunting direction because prior confirmed reports show repeated authorization boundary issues in GraphQL/JSON surfaces, but current evidence is still reconnaissance + negative controls.
