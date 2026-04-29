# HackerOne low-privilege organization API token boundary checks — 2026-04-29

## Scope / safety

- Target: owned HackerOne sandbox organization `info5995_bounty_sandbox_joey_2_demo` and owned program `info5995_bounty_sandbox_jo_h1b`.
- Purpose: compare an organization admin browser session with organization API tokens assigned to lower/no permissions.
- No token secret is stored in this evidence file.
- Created test tokens were revoked after testing.
- Requests were low-frequency, scoped to owned sandbox IDs only, and did not enumerate third-party data.

## Token creation attempts

### Failed long identifier attempt

Initial identifier `info5995-readonly-boundary-20260429` was rejected before token creation:

```json
{
  "was_successful": false,
  "unhashed_token": null,
  "errors": [{
    "field": "identifier",
    "message": "is too long (maximum is 30 characters)"
  }]
}
```

### Read-only token

Created organization API token:

- Identifier: `info5995-ro-20260429`
- Organization: `Z2lkOi8vaGFja2Vyb25lL09yZ2FuaXphdGlvbi83NTg0OQ==` / `Organization/75849`
- Group assignment: `OrganizationMemberGroup/355011` — `Program viewers for INFO5995 Bounty Sandbox Jo H1B`, key `read-only`
- Permissions on group:
  - program: `read_only_member`
  - inbox: `read_only_member`
- Created organization member:
  - GID: `Z2lkOi8vaGFja2Vyb25lL09yZ2FuaXphdGlvbk1lbWJlci8xMDMyMTQ0`
  - DB ID: `1032144`
  - user GID: `Z2lkOi8vaGFja2Vyb25lL1VzZXIvNDQ0MjcwOA==`
  - user DB ID: `4442708`
  - system user: `true`
  - organization admin: `false`

### No-group token

Created organization API token:

- Identifier: `info5995-nogroup-0429`
- Organization: `Z2lkOi8vaGFja2Vyb25lL09yZ2FuaXphdGlvbi83NTg0OQ==` / `Organization/75849`
- Group assignment: none (`[]`)
- Created organization member:
  - GID: `Z2lkOi8vaGFja2Vyb25lL09yZ2FuaXphdGlvbk1lbWJlci8xMDMyMTQ1`
  - DB ID: `1032145`
  - user GID: `Z2lkOi8vaGFja2Vyb25lL1VzZXIvNDQ0MjcxMA==`
  - user DB ID: `4442710`
  - system user: `true`
  - organization admin: `false`

## API behavior: read-only token

Authentication method: HTTP Basic auth using identifier `info5995-ro-20260429` plus generated token secret.

| Request | Status | Result |
|---|---:|---|
| `GET https://api.hackerone.com/v1/me` | `404` | API endpoint not available / not found for this token path |
| `GET https://api.hackerone.com/v1/hackers/programs/info5995_bounty_sandbox_jo_h1b` | `401` | Hacker-facing program API denied |
| `GET https://api.hackerone.com/v1/organizations/info5995_bounty_sandbox_joey_2_demo/reports` | `404` | Endpoint not available / not found |
| `POST https://hackerone.com/graphql` with Basic auth | `401` | `Invalid authentication token` |
| `GET https://api.hackerone.com/v1/reports/3702107` | `200` | Returned owned private report content |
| `GET https://api.hackerone.com/v1/reports/3701033` | `200` | Returned owned sandbox demo report content |
| `GET https://api.hackerone.com/v1/reports/3701035` | `200` | Returned owned sandbox demo report content |

For `GET /v1/reports/3702107`, response included:

- `id`: `3702107`
- `type`: `report`
- `title`: `INFO5995 sandbox auth boundary test report - safe owned data`
- `attributes.vulnerability_information`: present
- `relationships.reporter.data.attributes.username`: `joeyq`
- `relationships.program.data.attributes.handle`: `info5995_bounty_sandbox_jo_h1b`

For existing owned demo reports:

| Report ID | Status | Title | Reporter | Program | `vulnerability_information` present? |
|---|---:|---|---|---|---|
| `3701033` | `200` | `Demo report: XSS in INFO5995 Bounty Sandbox Jo H1B home page` | `demo-hacker` | `info5995_bounty_sandbox_jo_h1b` | yes |
| `3701035` | `200` | `Critical Authentication Bypass in Admin Panel` | `demo-hacker` | `info5995_bounty_sandbox_jo_h1b` | yes |

Interpretation: this appears consistent with the token being explicitly placed in the program viewer/read-only group. It is useful role-boundary evidence but not a confirmed vulnerability by itself.

## API behavior: no-group token

Authentication method: HTTP Basic auth using identifier `info5995-nogroup-0429` plus generated token secret.

| Request | Status | Result |
|---|---:|---|
| `GET https://api.hackerone.com/v1/reports/3702107` | `403` | Denied |
| `GET https://api.hackerone.com/v1/reports/3701033` | `403` | Denied |
| `GET https://api.hackerone.com/v1/hackers/programs/info5995_bounty_sandbox_jo_h1b` | `401` | Denied |

Interpretation: API token with no group membership could not access owned private reports. This is an important negative control.

## Cleanup / revocation

Both test tokens were revoked through GraphQL `DestroyOrganizationApiToken`:

| Identifier | OrganizationMember GID | Revocation result |
|---|---|---|
| `info5995-ro-20260429` | `Z2lkOi8vaGFja2Vyb25lL09yZ2FuaXphdGlvbk1lbWJlci8xMDMyMTQ0` | `was_successful: true` |
| `info5995-nogroup-0429` | `Z2lkOi8vaGFja2Vyb25lL09yZ2FuaXphdGlvbk1lbWJlci8xMDMyMTQ1` | `was_successful: true` |

Post-revocation verification:

| Identifier | Request | Status |
|---|---|---:|
| `info5995-ro-20260429` | `GET /v1/reports/3702107` | `401` |
| `info5995-nogroup-0429` | `GET /v1/reports/3702107` | `401` |

## Conclusion

No zero-day confirmed in this token pass.

What this did establish:

1. GraphQL does not accept organization API tokens via Basic auth (`Invalid authentication token`).
2. A token assigned to `read_only_member` can read private reports for its owned program through `api.hackerone.com/v1/reports/:id`.
3. A token with no group membership is denied for the same owned private report IDs.
4. Token cleanup succeeded and revoked tokens return `401`.

Next promising direction: create or use a genuinely separate low-privilege human/hacker account or collaborator-invite flow, then test report access after invite acceptance, rejection, revocation, and leaving the report. That is more likely to expose stale authorization than org API token group boundaries.
