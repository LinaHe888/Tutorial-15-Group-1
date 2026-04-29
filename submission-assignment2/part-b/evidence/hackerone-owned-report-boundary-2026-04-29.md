# HackerOne owned sandbox report boundary checks — 2026-04-29

## Scope / safety

- Target: HackerOne owned bug bounty sandbox program `info5995_bounty_sandbox_jo_h1b` under Joey's owned sandbox organization.
- Testing stayed within owned/sandbox data.
- Frequency: single-digit manual requests; no scanning, no brute force, no ID enumeration.

## Setup

Created a controlled sandbox report via the HackerOne UI:

- Report ID: `3702107`
- Title: `INFO5995 sandbox auth boundary test report - safe owned data`
- Reporter: `joeyq`
- Team/program: `info5995_bounty_sandbox_jo_h1b`
- Purpose: create owned private report data for authz boundary checks only.

The inbox also displayed existing owned sandbox reports:

- `3701035` — reporter `demo-hacker`
- `3701033` — reporter `demo-hacker`

## Authenticated positive control

Authenticated as `joeyq` in the sandbox/org-admin browser session.

### `/reports/3702107.json`

- Status: `200`
- Content-Type: `application/json; charset=utf-8`
- High-level keys included expected private report representation keys: `id`, `global_id`, `url`, `title`, `state`, `reporter`, `team`, `can_view_report`, `visibility`, `vulnerability_information`, `weakness`, `attachments`, `abilities`, `summaries`.
- Sensitive-key quick scan found no obvious email/phone/token/password leak paths. Only matched paths:
  - `credential_account_details = null`
  - `abilities.can_view_credential_account_details? = true`

### GraphQL `report(id:)` / `reports(database_id:)`

Authenticated GraphQL returned the expected owned private report content:

```graphql
query($id:Int!){
  report(id:$id){
    id _id title state vulnerability_information impact
    reporter{username id}
    team{handle id}
  }
}
```

Result summary:

- `_id`: `3702107`
- title returned correctly
- private `vulnerability_information` returned correctly
- reporter username `joeyq` returned correctly
- team handle `info5995_bounty_sandbox_jo_h1b` returned correctly

## Unauthenticated negative controls

No cookies / unauthenticated HTTP client.

### `/reports/3702107.json`

- Status: `404`
- Body: empty text response
- No private report content leaked.

### `/reports/3702107`

- Status: `302`
- Location: `https://hackerone.com/users/sign_in`
- No private report content leaked.

### GraphQL `report(id:)`

Returned `NOT_FOUND`:

```json
{
  "errors": [{"message": "Report does not exist", "type": "NOT_FOUND"}],
  "data": {"report": null}
}
```

### GraphQL `reports(database_id:)`

Returned empty edges:

```json
{"data":{"reports":{"edges":[]}}}
```

### GraphQL `node(id:)` with `gid://hackerone/Report/3702107`

Returned `NOT_FOUND` and `node: null`.

### `/bugs.json` scoped to owned program/report

- GET `/bugs.json?...report_id=3702107...`: `401`, JSON body `{ "error": "You need to sign in or sign up before continuing." }`
- POST same path without CSRF: `412`, empty text body
- No private report content leaked.

## Conclusion

The owned-report public/unauthenticated boundary behaved correctly for this pass. No zero-day candidate confirmed here.

Best next step: continue with authenticated role-boundary testing using a lower-privilege owned account/session (`demo-hacker` or `demo-triager`) to test whether non-admin / revoked collaborator / cross-program contexts can access report JSON, GraphQL `report(id:)`, attachments, participant/invitation state, or `/bugs.json` inbox filters unexpectedly.

## Additional unauthenticated checks on existing owned sandbox reports

Existing owned sandbox reports shown in the org inbox were also checked without cookies:

| Report ID | `/reports/:id.json` | GraphQL `report(id:)` | Result |
|---|---:|---:|---|
| `3701033` | `404` | `NOT_FOUND`, `report: null` | No leak |
| `3701035` | `404` | `NOT_FOUND`, `report: null` | No leak |

These were not sequentially enumerated; they were IDs visible in the owned sandbox inbox.

## Duplicate/collaborator GraphQL boundary spot-checks

Motivation: historical HackerOne issues included GraphQL overexposure through alternate report query paths, including duplicate/collaboration workflows.

Authenticated as `joeyq` / org admin:

- `CloseAsDupeModal`-style query for `triagedReportId=3702107`, `selectedReportId=3701033` returned expected owned-sandbox report metadata.
- That positive control includes `reporter.email` for the current user's own report; email value was redacted in notes and treated as expected admin-visible data.
- `ReportCollaboratorQuery(reportId: 3702107)` returned the owned report and empty collaborator/invitation collections:
  - `report_collaborators.total_count = 0`
  - `report_collaborator_invitations.total_count = 0`

Unauthenticated / no-cookie controls:

- `CloseAsDupeModal`-style query returned `me: null`, `triagedReport.nodes: []`, and `selectedReport.nodes: []`.
- `ReportCollaboratorQuery(reportId: 3702107)` returned `NOT_FOUND` with `report: null`.

Conclusion: no unauthenticated exposure found through these alternate report GraphQL paths.
