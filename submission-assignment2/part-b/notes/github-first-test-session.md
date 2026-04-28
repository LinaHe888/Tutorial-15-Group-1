# GitHub First Test Session Plan

Goal: run a first low-risk session against owned GitHub assets and produce candidate evidence for C1.

Status: not executed yet.

## Pre-flight

- [ ] Read GitHub Bug Bounty scope/rules pages again and capture screenshots/PDFs.
- [ ] Confirm test org and repos are team-owned.
- [ ] Confirm Account B role before each test.
- [ ] Confirm tokens are fine-grained/minimal and stored only in environment variables.
- [ ] Create evidence folder: `submission-assignment2/part-b/evidence/github-c1/`.

## Test C1-GH-001 — Account B cannot read private repo B as outsider

Hypothesis: if Account B has no access to repo B but can read repo B metadata/content/issues/artifacts, this is an access-control vulnerability.

Setup:
- Account A owns org/repo B.
- Repo B is private.
- Account B has no access to repo B.

Expected:
- REST and GraphQL return 404/403/null for private repo B resources.

Commands to run only after env vars are set:

```bash
# REST: repo metadata as Account B
curl -sS -i \
  -H "Authorization: Bearer $GH_ACCOUNT_B_TOKEN" \
  -H "Accept: application/vnd.github+json" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  "https://api.github.com/repos/$GH_TEST_ORG/$GH_REPO_B"

# REST: repo contents as Account B
curl -sS -i \
  -H "Authorization: Bearer $GH_ACCOUNT_B_TOKEN" \
  -H "Accept: application/vnd.github+json" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  "https://api.github.com/repos/$GH_TEST_ORG/$GH_REPO_B/contents/README.md"
```

Keep if:
- B receives data that should require repo access.

Drop if:
- B receives expected 404/403/null.

## Test C1-GH-002 — GraphQL node ID authorization check

Hypothesis: if Account B can query a private issue/repo node ID from repo B without access, private metadata may leak.

Setup:
- Account A creates a private issue in repo B with fake title `PRIVATE-ISSUE-TITLE-FAKE-12345`.
- Account A records the issue `node_id` from REST or GraphQL.
- Account B has no access to repo B.

Expected:
- Account B cannot retrieve title/body/metadata.

Query template:

```bash
export GH_PRIVATE_NODE_ID='NODE_ID_FROM_ACCOUNT_A'

curl -sS -i https://api.github.com/graphql \
  -H "Authorization: Bearer $GH_ACCOUNT_B_TOKEN" \
  -H "Content-Type: application/json" \
  --data @- <<'JSON'
{
  "query": "query($id: ID!) { node(id: $id) { __typename ... on Issue { title body repository { nameWithOwner isPrivate } } ... on Repository { nameWithOwner isPrivate } } rateLimit { cost remaining } }",
  "variables": { "id": "REPLACE_WITH_NODE_ID" }
}
JSON
```

Before running, replace `REPLACE_WITH_NODE_ID` with `$GH_PRIVATE_NODE_ID` safely or use a small local script that substitutes it. Do not publish the node ID if it could expose real assets; for team-owned fake assets it is OK to store redacted evidence.

Keep if:
- B sees title/body/repo metadata for private data.

Drop if:
- B receives `null`, permission error, or no sensitive fields.

## Test C1-GH-003 — GitHub App installed on repo A cannot read repo B

Hypothesis: a GitHub App installation token scoped only to repo A must not access repo B.

Setup:
- Account A creates a GitHub App with minimal read permissions.
- Install the app only on repo A.
- Generate an installation token for the app.

Expected:
- App token can read allowed repo A if permission permits.
- App token cannot read repo B.

Evidence:
- screenshot of installation scope showing only repo A;
- API response for repo A;
- API response for repo B denied.

Keep if:
- app token reads repo B.

Drop if:
- repo B is denied.

## Test C1-GH-004 — Write/maintain role cannot change admin-only settings

Hypothesis: Account B with write/maintain access to repo A must not make repo A public, alter branch protection, change secrets, or install elevated apps if those require admin.

Setup:
- Account B has write or maintain access to repo A, not admin.
- Repo A is private and has a protected branch.

Expected:
- UI and API deny admin-only operations.

Safe API examples to check permissions before attempting state-changing actions:

```bash
# Check B's repository permission view
curl -sS -i \
  -H "Authorization: Bearer $GH_ACCOUNT_B_TOKEN" \
  -H "Accept: application/vnd.github+json" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  "https://api.github.com/repos/$GH_TEST_ORG/$GH_REPO_A"
```

Do not run destructive/state-changing actions unless the team deliberately chooses them on the owned repo and captures before/after evidence.

Keep if:
- B can perform an admin-only operation without admin permission.

Drop if:
- denied as expected.

## Test C1-GH-005 — Actions token cannot access repo B artifacts/content

Hypothesis: `GITHUB_TOKEN` from repo A workflow must not access private repo B unless explicitly authorized.

Setup:
- Repo A has a minimal workflow.
- Repo B is private.
- Workflow uses only the default `GITHUB_TOKEN` from repo A.

Expected:
- Calls from repo A workflow to repo B content/artifacts are denied.

Important:
- Keep workflow minimal.
- Do not loop or scan.
- One run is enough for evidence.

Keep if:
- repo A `GITHUB_TOKEN` reads repo B private content/artifacts.

Drop if:
- denied as expected.

## Recording candidate outcomes

For each test, copy `github-test-case-template.md` and fill:
- exact setup;
- expected vs actual;
- redacted API responses/screenshots;
- severity mapping;
- novelty search.

After first session, update `candidate-findings.md` with Keep/Drop decisions.
