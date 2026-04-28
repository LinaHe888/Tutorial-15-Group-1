# Account B Boundary Tests — 2026-04-28

Account B: `Joey-QIAO-debug`

Setup confirmed:
- Repo A: `HeadmasterEggy/info5995-a2-private-alpha`, private, Account B has `read` / `pull` only.
- Repo B: `HeadmasterEggy/info5995-a2-private-beta`, private, Account B has `none`.

Tests executed:
1. Account B can read repo A metadata and README.
2. Account B cannot read repo B metadata, README, issue, or artifacts.
3. Account B cannot resolve repo B fake private issue via GraphQL node ID.
4. Account B cannot access repo A admin-style endpoints: invitations, collaborators, actions secrets.

Evidence files:
- `008-account-b-gh-auth-status-redacted.txt`
- `009-account-b-viewer.json`
- `010-owner-view-account-b-alpha-permission.json`
- `011-owner-view-account-b-beta-permission.json`
- `012-account-b-alpha-repo-readable.json`
- `013-account-b-alpha-readme-readable.json`
- `014-account-b-denied-*`
- `015-owner-beta-fake-issue-node-redacted.json`
- `016-account-b-graphql-beta-issue-node-denied.txt`
- `017-account-b-alpha-admin-denied-*`

Result:
- No vulnerability found in this Account B pass.
- Permission boundaries behaved as expected.
- This still provides useful baseline evidence for scope, methodology, and negative controls.

Next recommended path:
- Either escalate Account B to `write` on repo A and test write-vs-admin boundaries, or switch to a richer target surface such as GitHub App installation scoped to repo A only.
