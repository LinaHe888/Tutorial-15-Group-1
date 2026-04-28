# GitHub Owned Assets Setup Checklist

Purpose: prepare a legal, in-scope, low-risk environment for Assignment 2 Part B GitHub Bug Bounty testing.

Do **not** test other users' repositories, organizations, data, or PII. Every test below must use only team-owned accounts/assets.

## Minimum assets needed

| Asset | Suggested name | Owner | Purpose |
|---|---|---|---|
| Account A | existing primary account or new test account | team | owner/admin role |
| Account B | new test account | team | low-privilege / outsider / collaborator role |
| Test org | `info5995-a2-bounty-<team>` | Account A | org/repo permission tests |
| Private repo A | `a2-private-alpha` | test org | protected/private resource |
| Private repo B | `a2-private-beta` | test org | cross-repo boundary target |
| Optional GitHub App | `INFO5995 A2 Scope Test` | Account A/org | app installation scope tests |
| Optional OAuth App | `INFO5995 A2 OAuth Test` | Account A | OAuth scope tests |

## Account/role setup

1. Account A creates the test organization.
2. Account A creates two private repositories in the org:
   - `a2-private-alpha`
   - `a2-private-beta`
3. Add harmless dummy content only:
   - `README.md`
   - `dummy-secret.txt` containing fake text like `FAKE_TEST_SECRET_DO_NOT_USE`
   - one private issue with a clearly fake title, e.g. `PRIVATE-ISSUE-TITLE-FAKE-12345`
4. Invite Account B with controlled roles for different tests:
   - outsider/no access
   - read collaborator on repo A only
   - write/maintain collaborator on repo A only
   - never org owner/admin except for explicit negative-control tests

## Tokens needed for API tests

Create only minimal-scope tokens. Store them locally, never commit them.

Recommended environment variables:

```bash
export GH_ACCOUNT_A_TOKEN='...'
export GH_ACCOUNT_B_TOKEN='...'
export GH_TEST_ORG='info5995-a2-bounty-TEAM'
export GH_REPO_A='a2-private-alpha'
export GH_REPO_B='a2-private-beta'
```

Token guidance:
- Account A token: enough to create/read owned test data, ideally fine-grained to the test org/repos only.
- Account B token: intentionally limited to the role being tested.
- For a negative test, B should be missing the permission being checked.

## Evidence storage

Use this structure:

```text
submission-assignment2/part-b/evidence/github-c1/
  scope-screenshots/
  setup-screenshots/
  api-responses-redacted/
  test-notes/
```

Do not store raw tokens, cookies, session IDs, or real PII.

## Safety boundaries

Stop immediately if:
- any response contains data outside team-owned assets;
- a test would require guessing real usernames/repo names/node IDs;
- a test needs high-volume requests;
- a test might degrade availability or consume excessive resources.

If a finding appears higher than Medium severity, follow the assignment guidance: sanity-check with tutor/coordinator before broad external submission.
