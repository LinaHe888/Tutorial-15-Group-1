# Account B Write Boundary Tests — 2026-04-28

Account B: `Joey-QIAO-debug`

Setup confirmed:
- Repo A: `HeadmasterEggy/info5995-a2-private-alpha`, private, Account B upgraded to `write` / `push`.
- Repo B: `HeadmasterEggy/info5995-a2-private-beta`, private, Account B remains `none`.

Tests executed:
1. Account B can view repo A with write permissions.
2. Account B can create/update harmless file `account-b-write-proof.txt` in repo A.
3. Account B still cannot read repo B metadata/content/artifacts.
4. Account B cannot access repo A invitations, deploy keys, or branch protection.
5. Account B with write access can list repo A Actions secret metadata/name, including fake secret `INFO5995_FAKE_SECRET_NAME`; no secret values were exposed.

Result:
- No bounty-grade vulnerability found in write-collaborator pass.
- Expected/benign behavior observed: write access permits code changes in repo A, but does not grant repo B access.
- Interesting behavior for presentation notes: write collaborator can enumerate Actions secret names/metadata, but not values. Treat as likely intended GitHub behavior unless documentation says otherwise.

Next recommended path:
- Build a GitHub App installed only on repo A and test whether app installation/user tokens can cross into repo B.
- If GitHub App flow is too slow, switch to OpenAI/Microsoft backup target or convert these negative controls into methodology slides while continuing hunting.
