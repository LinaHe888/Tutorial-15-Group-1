# HackerOne GraphQL authz variant probes — 2026-04-29

## Context

After public HackerOne report `#3287208` disclosed a GraphQL mutation-aliasing DoS in HackerOne’s account-recovery phone verification flow, I treated GraphQL as a useful variant-hunting surface but avoided DoS-style testing. Testing below stayed low-frequency and read-only except for one owned sandbox report-intent draft that was immediately deleted.

Scope basis: HackerOne’s own program at `https://hackerone.com/security` lists `hackerone.com` / GraphQL as in-scope.

## Safety limits

- No scanning, brute force, or high-volume aliasing.
- No DoS reproduction.
- No private third-party report probing beyond a few known IDs used as negative controls.
- Only one owned sandbox `ReportIntent` was created and then deleted.

## Browser-authenticated GraphQL setup

GraphQL requests sent from the authenticated browser context required the page CSRF token:

- `x-csrf-token: <meta name="csrf-token">`
- `credentials: include`

A simple authenticated control query returned current user `joeyq`, confirming authenticated browser GraphQL execution.

## Probe 1 — Query aliasing timing sanity check

Read-only `node(id:)` query aliased 1 / 2 / 3 times using a known owned/user node ID.

Observed timings:

- 1 alias: ~3.27s
- 2 aliases: ~3.33s
- 3 aliases: ~3.29s

Result: no linear timing increase for this read-only query. No DoS-style follow-up performed.

## Probe 2 — Team invitation authorization boundary

Operation reconstructed from JS bundle: `TeamInvitations`.

Query target fields included:

- `team(handle)`
- `can_update_all_hacker_invitations`
- `pending_invitations.total_count`
- `pending_invitations.edges.node.email`
- recipient user fields

Targets and results:

| Handle | Result |
|---|---|
| `info5995_bounty_sandbox_jo_h1b` | owned sandbox returned team data; `can_update_all_hacker_invitations: true`; no pending invitations |
| `info5995_bounty_sandbox_joey_2_demo` | `Team does not exist` for this handle |
| `security` | public program returned team metadata; `can_update_all_hacker_invitations: false`; `pending_invitations.total_count: 0`; no invitation edges |
| `neon_bbp` | public program returned team metadata; `can_update_all_hacker_invitations: false`; no invitation edges |
| `alsco` | public program returned team metadata; `can_update_all_hacker_invitations: false`; no invitation edges |

Assessment: no invitation email/token leakage confirmed. Public programs exposed non-sensitive team metadata only.

## Probe 3 — Report collaborator / invitation authorization boundary

Operation reconstructed from JS bundle: `ReportCollaboratorQuery`.

Query target fields included:

- `report(id)`
- `report_collaborators.total_count`
- collaborator `user.id` / `username`
- `report_collaborator_invitations.total_count`
- invitation `state`, `email`, `recipient`, `bounty_weight`

Report IDs tested:

| Report ID | Result |
|---|---|
| `3702107` | owned sandbox report returned; no collaborators/invitations |
| `3701033` | owned/demo sandbox report returned; no collaborators/invitations |
| `3701035` | owned/demo sandbox report returned; no collaborators/invitations |
| `3641166` | negative control from hacktivity; `Report does not exist` |
| `3695031` | negative control from hacktivity; `Report does not exist` |
| `3168691` | public disclosed report returned public title + no collaborators/invitations |

Assessment: no collaborator/invitation leakage confirmed. Private/undisclosed report IDs returned `NOT_FOUND`; public disclosed report returned only expected public report data.

## Probe 4 — Current user collaboration invitations

Operation: `MyCollaborationInvites`.

Result:

```json
{
  "me": {
    "username": "joeyq",
    "collaboration_invitations": { "nodes": [] }
  }
}
```

Assessment: no stale or unexpected collaborator invitations present.

## Probe 5 — ReportIntent IDOR boundary using owned sandbox draft

Operation used to create/resume draft: `SubmitWithHaiButton` / `resumeOrCreateReportIntent`.

Team ID:

```text
Z2lkOi8vaGFja2Vyb25lL0VuZ2FnZW1lbnRzOjpCdWdCb3VudHlQcm9ncmFtLzExMjMyMQ==
```

Created owned sandbox draft:

```json
{
  "report_intent": {
    "id": "Z2lkOi8vaGFja2Vyb25lL1JlcG9ydEludGVudC8xMTUxNDg=",
    "databaseId": "115148"
  },
  "was_successful": true
}
```

Read-own control:

- `ReportIntentV2Query(id: 115148)` returned own sandbox draft:
  - title: `Report Intent #115148`
  - state: `open`
  - team: `info5995_bounty_sandbox_jo_h1b`
  - report: `null`
  - attachments: `[]`

Neighbor / negative controls:

| ID | Result |
|---|---|
| `115147` | `report_intent: null` |
| `115149` | `report_intent: null` |
| `1` | `report_intent: null` |
| `99999999` | `report_intent: null` |

Cleanup:

```json
{
  "deleteReportIntent": {
    "was_successful": true,
    "errors": { "edges": [] }
  }
}
```

Post-delete verification:

- `ReportIntentV2Query(id: 115148)` returned `report_intent: null`.

Assessment: no ReportIntent IDOR confirmed. Owned draft lifecycle behaved correctly and was cleaned up.

## Current conclusion

No confirmed zero-day from this GraphQL authz pass.

Best remaining HackerOne path is still collaborator invite/revoke stale authorization, but that requires a second account or real collaborator invitation flow. Without a second account, current tests can only confirm same-user / owned-resource boundaries.
