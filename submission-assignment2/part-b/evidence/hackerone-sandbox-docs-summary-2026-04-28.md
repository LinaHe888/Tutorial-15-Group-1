# HackerOne sandbox / test environment docs summary — 2026-04-28

Public-docs finding only; no active testing performed for this item.

## Conclusion

HackerOne documents an API sandbox/test program path, but it appears to be customer/program-side rather than a standalone hacker-only sandbox for arbitrary owned reports/programs.

## Key URLs

- Official API sandbox docs: https://api.hackerone.com/getting-started/
  - Mentions a sandbox for customers to test API functionality.
  - Links to `https://hackerone.com/teams/new/sandbox`.
  - Requires being logged in / creating an account.
  - Says users can select product edition and get access to most features.
- Customer API use cases: https://api.hackerone.com/use-cases/
  - API is framed around customers interacting with report/program data.
- Organization API tokens: https://docs.hackerone.com/organizations/api-tokens.html
  - Program administrative users create/manage organization API tokens; token access follows org/group permissions.
- Hacker API docs: https://api.hackerone.com/getting-started-hacker-api/
  - Personal hacker API exists for reports/programs/bounties/earnings.
  - No separate hacker-only sandbox found.
- Hacker API create report endpoint: https://api.hackerone.com/hacker-resources/
  - Documents `POST /v1/hackers/reports`, which submits to a `team_handle` / `structured_scope_id`; it does not create an owned program.
- Training/lab sandboxes: https://www.hackerone.com/blog/test-your-hacking-skills-real-world-simulated-bugs
  - Training environments, not HackerOne platform sandboxes for owning programs/reports.

## Practical implication for Part B

To test the strongest historical variant class safely, we likely need to create/use the documented customer/program sandbox (`/teams/new/sandbox`) so we can own both sides of the authorization boundary. A normal hacker login alone currently shows no private programs and cannot create arbitrary owned programs from the standard hacker UI.
