# Braze quick scope + public probes — 2026-04-29

## Why selected

Braze looked promising for a quick pass because its HackerOne program exposes a dedicated bug-bounty dashboard/API environment rather than production customer assets.

Program: `https://hackerone.com/braze_inc?type=team`

In-scope assets captured from policy scopes:

1. `https://bug-bounty-rest.k8s.tools-001.d-use-1.braze-dev.com/`
   - REST API for dashboard management.
   - Public docs: `https://www.braze.com/docs/api/basics`

2. `https://bug-bounty-dashboard.k8s.tools-001.d-use-1.braze-dev.com/`
   - Dashboard URL.
   - Program note: only test accounts you own or have explicit permission to test.

3. `https://bug-bounty-api.k8s.tools-001.d-use-1.braze-dev.com/`
   - Non-REST API / dashboard-backed API.

## Public unauthenticated baseline

### Dashboard

`GET https://bug-bounty-dashboard.k8s.tools-001.d-use-1.braze-dev.com/`

- Status: `200`
- Final URL: `/sign_in`
- Title: `Log in to Braze | Braze`
- Set cookie:
  - `_session_id=...; secure; HttpOnly; SameSite=None`
- Security headers observed:
  - `X-Frame-Options: SAMEORIGIN`
  - `Strict-Transport-Security: max-age=31536000; includeSubDomains`

Login form observed:

```html
<form class="form-stacked container" id="email-form" method="post" action="/clusters">
  <input type="hidden" name="store_developer_location" value="" data-parsley-required/>
  <input type="email" id="developer-email" name="email" placeholder="Enter your email address" data-parsley-required/>
</form>
```

A single owned/invalid email check using `info5995-test@example.invalid` posted to `/clusters` returned the normal auth page at `/auth?origin=`; no obvious cluster/user enumeration was observed in the sampled response.

### API roots

`GET https://bug-bounty-api.k8s.tools-001.d-use-1.braze-dev.com/`

- Status: `400`
- Body: `Did you mean to visit the <a href="https://bug-bounty-dashboard.k8s.tools-001.d-use-1.braze-dev.com/dashboard">Braze dashboard</a>?`

`GET https://bug-bounty-rest.k8s.tools-001.d-use-1.braze-dev.com/`

- Status: `400`
- Same dashboard hint body.

## Current assessment

No confirmed zero-day from public/no-account probes.

Braze remains a reasonable candidate if we create/obtain an owned bug-bounty dashboard account, because the dedicated test dashboard/API likely supports owned-account authz testing. Without credentials, only superficial public checks are possible.
