# Neon quick scope + public probes — 2026-04-29

## Why selected

Neon looked like a strong simple target candidate from HackerOne opportunities because it has:

- only three in-scope assets;
- explicit staging environment;
- public preview code for staging;
- console/API surfaces suitable for authorization-boundary testing;
- self-service signup instructions in the program text.

## Scope captured

Program: `https://hackerone.com/neon_bbp?type=team`

Scope notes from `https://hackerone.com/neon_bbp/policy_scopes`:

- “You can sign up at `https://console.neon.tech/` and `https://console-stage.neon.build/`.”
- “Please use `@wearehackerone.com` emails to help us identify testing activities.”
- Staging preview code: `I-LOVE-PREVIEWS`
- Staging invite link: `https://console-stage.neon.build/?invite=I-LOVE-PREVIEWS`

In-scope assets:

1. `https://console.neon.tech/api/v2/`
   - Type: URL
   - Coverage: In scope
   - Max severity: Critical
   - Bounty eligible

2. `https://console.neon.tech/`
   - Type: URL
   - Coverage: In scope
   - Max severity: Critical
   - Bounty eligible

3. `https://console-stage.neon.build/`
   - Type: URL
   - Coverage: In scope
   - Max severity: Critical
   - Bounty eligible

## Public unauthenticated baseline

### `https://console.neon.tech/`

- Redirects into Keycloak/OIDC login flow:
  - Realm: `prod-realm`
  - Client: `neon-console`
  - Redirect URI: `https://console.neon.tech/auth/keycloak/callback`
- Security headers observed on login response:
  - `content-security-policy: frame-src 'self'; frame-ancestors 'self'; object-src 'none';`
  - `x-frame-options: SAMEORIGIN`
  - `strict-transport-security: max-age=31536000; includeSubDomains`
- Auth cookie example attributes:
  - `Secure`
  - `HttpOnly`
  - `SameSite=None`

### `https://console-stage.neon.build/`

- Redirects into Keycloak/OIDC login flow:
  - Realm: `staging-realm`
  - Client: `neon-console`
  - Redirect URI: `https://console-stage.neon.build/auth/keycloak/callback`
- Security headers equivalent to production login.

### `https://console-stage.neon.build/?invite=I-LOVE-PREVIEWS`

- Still redirects into staging Keycloak/OIDC login flow.
- No unauthenticated data exposure observed in the first response.

### `https://console.neon.tech/api/v2/`

Unauthenticated baseline:

```json
{
  "request_id": "787531a0-ac10-4f2c-aabf-416c375e6d0b",
  "code": "",
  "message": "supplied credentials do not pass authentication"
}
```

- Status: `401`
- No public data returned.

## OIDC / redirect quick checks

OIDC discovery endpoints worked as expected:

- `https://console.neon.tech/realms/prod-realm/.well-known/openid-configuration`
- `https://console-stage.neon.build/realms/staging-realm/.well-known/openid-configuration`

Invalid redirect URI checks against both prod and staging auth endpoints returned `400` with:

```json
{"error":"Invalid parameter: redirect_uri"}
```

Rejected examples:

- `https://example.com/`
- `https://console.neon.tech.evil.example/callback`
- `//example.com/cb`

No open redirect confirmed.

## CORS quick checks

Preflight `OPTIONS` requests to:

- `https://console.neon.tech/api/v2/`
- `https://console.neon.tech/api/v2/projects`
- `https://console-stage.neon.build/api/v2/`
- `https://console-stage.neon.build/api/v2/projects`

Origins checked:

- `https://evil.example`
- `null`
- `https://console.neon.tech`
- `https://console-stage.neon.build`

Observed behavior:

- Status: `204`
- `vary: Origin, Access-Control-Request-Method, Access-Control-Request-Headers`
- No permissive `Access-Control-Allow-Origin` header observed for tested origins.

No CORS misconfiguration confirmed.

## Current assessment

No confirmed zero-day from public/no-account probes.

Neon still looks promising, but meaningful testing likely requires creating/logging into owned test accounts and checking project/org/API authorization boundaries in prod/staging. That is an external account/action step and should be done deliberately with clear test identities.
