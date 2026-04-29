# HackerOne owned organization `node(id:)` boundary checks — 2026-04-29

## Scope / safety

- Target: owned HackerOne sandbox organization `info5995_bounty_sandbox_joey_2_demo` and its owned sandbox program `info5995_bounty_sandbox_jo_h1b`.
- Motivation: historical HackerOne report `#1618347` showed GraphQL GID `node(id:)` authorization bypass around private program policy/asset data.
- No enumeration: IDs below came from the authenticated owned organization view only.
- No mutation / no destructive change.

## Authenticated positive controls (`joeyq` org admin)

Authenticated GraphQL could resolve expected owned objects through `node(id:)`:

| Label | GID | Type | Key returned fields |
|---|---|---|---|
| Organization | `Z2lkOi8vaGFja2Vyb25lL09yZ2FuaXphdGlvbi83NTg0OQ==` | `Organization` | `_id=75849`, handle `info5995_bounty_sandbox_joey_2_demo`, `i_am_organization_admin=true` |
| Program | `Z2lkOi8vaGFja2Vyb25lL0VuZ2FnZW1lbnRzOjpCdWdCb3VudHlQcm9ncmFtLzExMjMyMQ==` | `Team` | `_id=112321`, handle `info5995_bounty_sandbox_jo_h1b`, `i_can_view_reports=true`, `i_can_manage_program=true` |
| Read-only member group | `Z2lkOi8vaGFja2Vyb25lL09yZ2FuaXphdGlvbk1lbWJlckdyb3VwLzM1NTAxMQ==` | `OrganizationMemberGroup` | `_id=355011`, key `read-only`, permissions include `read_only_member` |
| Demo triager org member | `Z2lkOi8vaGFja2Vyb25lL09yZ2FuaXphdGlvbk1lbWJlci8xMDMxNjkw` | `OrganizationMember` | `_id=1031690`, `organization_admin=false`, username `demo-triager` |
| Default inbox | `Z2lkOi8vaGFja2Vyb25lL09yZ2FuaXphdGlvbkluYm94LzEwODM5OQ==` | `OrganizationInbox` | `_id=108399`, handle `info5995_bounty_sandbox_jo_h1b_inbox`, `inbox_type=default` |

Additional owned-org objects observed during positive control:

- `OrganizationMemberGroup/355009` — `Program admins for INFO5995 Bounty Sandbox Jo H1B`, key `admin`.
- `OrganizationMemberGroup/355010` — `Report managers for INFO5995 Bounty Sandbox Jo H1B`, key `standard`.
- `OrganizationMemberGroup/355011` — `Program viewers for INFO5995 Bounty Sandbox Jo H1B`, key `read-only`.
- `OrganizationMemberGroup/355012` — `Intake Agent`, key `agent`.
- `OrganizationMember/1031688` — user `joeyq`, org admin.
- `OrganizationMember/1031689` — user `demo-member`, org admin.
- `OrganizationMember/1031690` — user `demo-triager`, not org admin.
- `OrganizationMember/1031691` — user `intake-agent-75849`, not org admin.

## Unauthenticated `node(id:)` controls

The same GIDs were queried without cookies using a single low-frequency GraphQL `node(id:)` request per object type. Results:

| Label | Unauthenticated result |
|---|---|
| Organization | `Organization does not exist`, `type=NOT_FOUND`, `node=null` |
| Program | `Engagements::BugBountyProgram does not exist`, `type=NOT_FOUND`, `node=null` |
| Read-only member group | `OrganizationMemberGroup does not exist`, `type=NOT_FOUND`, `node=null` |
| Demo triager org member | `OrganizationMember does not exist`, `type=NOT_FOUND`, `node=null` |
| Default inbox | `OrganizationInbox does not exist`, `type=NOT_FOUND`, `node=null` |

## Conclusion

The unauthenticated `node(id:)` boundary behaved correctly for these owned organization/program/member-group/member/inbox object types. No zero-day candidate confirmed here.

Useful next step: if Joey wants to go deeper, use an actual low-privilege owned session/API token for `demo-triager`, `demo-member`, or a newly invited secondary account. The current admin session can confirm expected data, but it cannot prove low-privilege denial/allow behavior.
