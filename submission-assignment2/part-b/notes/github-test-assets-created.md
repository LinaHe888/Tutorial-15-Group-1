# GitHub Test Assets Created

Created on 2026-04-28 by Goose using Joey's authenticated GitHub account `HeadmasterEggy`.

## Assets

| Type | URL | Visibility | Notes |
|---|---|---|---|
| Repo A | https://github.com/HeadmasterEggy/info5995-a2-private-alpha | Private | Contains a manual-only workflow that uploads a fake artifact. |
| Repo B | https://github.com/HeadmasterEggy/info5995-a2-private-beta | Private | Contains fake private test data and fake private issue. |

## Org status

A separate test organization was **not** created automatically because GitHub org creation is generally a browser/email-verification flow, not a reliable CLI/API operation.

For the first session, the tests used user-owned private repositories as a safe fallback. This still supports outsider/private-resource boundary checks. To test org-role boundaries, the team should either:

1. create a dedicated test org manually, or
2. invite a true Account B collaborator to repo A only and repeat cross-repo boundary tests.

## Fake markers

- Fake issue title: `PRIVATE-ISSUE-TITLE-FAKE-12345`
- Fake artifact name: `fake-info5995-artifact`
- Fake file marker: `FAKE_TEST_SECRET_DO_NOT_USE`

These are deliberately fake and safe to screenshot/present after redaction of unrelated account metadata.
