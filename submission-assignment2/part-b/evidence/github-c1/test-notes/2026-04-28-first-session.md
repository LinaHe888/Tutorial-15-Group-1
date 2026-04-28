# First GitHub C1 Test Session — 2026-04-28

Assets created:
- Private repo A: https://github.com/HeadmasterEggy/info5995-a2-private-alpha
- Private repo B: https://github.com/HeadmasterEggy/info5995-a2-private-beta
- Fake private issue title: PRIVATE-ISSUE-TITLE-FAKE-12345
- Fake workflow artifact name: fake-info5995-artifact

Tests executed:
1. Authenticated owner can read private repo B and fake issue.
2. Unauthenticated outsider cannot read private repo B metadata/content/issue.
3. Authenticated owner can list private repo A artifact after manual workflow run.
4. Unauthenticated outsider cannot list private repo A artifacts.

Result:
- No vulnerability found in this first baseline pass.
- Access control behaved as expected: private resources denied to unauthenticated outsider.

Next recommended step:
- Add a true Account B collaborator with limited read/write access to repo A only, then repeat boundary tests against repo B and admin-only settings.
