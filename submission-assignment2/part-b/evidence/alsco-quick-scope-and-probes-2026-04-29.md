# ALSCO quick scope + low-risk probes — 2026-04-29

## Why selected

ALSCO looked suitable for a quick INFO5995 zero-day pass because the HackerOne program has only two in-scope sandbox domains, high response efficiency, and explicitly scoped sandbox/CMS testing assets.

## Scope captured

Program: `https://hackerone.com/alsco?type=team`

In-scope assets from `https://hackerone.com/alsco/policy_scopes`:

1. `sandbox.securegateway.com`
   - Type: Domain
   - Coverage: In scope
   - Max severity: Critical
   - Bounty eligible
   - Program text mentions testing Secure Gateway 2FA, upload prevention, and content detector behavior.
   - Instructions say a Secure Gateway mobile app/test account is needed for 2FA testing.

2. `sandbox-royal.securegateway.com`
   - Type: Domain
   - Coverage: In scope
   - Max severity: Critical
   - Bounty eligible
   - Program text mentions Royal CMS checks against common injection classes: XSS, SQLi, OS/command injection, URL injection, RCE, privilege escalation.

Important restrictions / quirks:

- They say only “full hack scenario” reports are accepted, e.g. editing index page or downloading database.
- HTML upload containing JavaScript alone is not considered a vulnerability unless it changes major system content.
- Recorded video required for bounty reports.
- I kept testing to low-frequency manual GET/POST requests only; no brute force, no scanner, no DoS, no destructive action.

## Baseline observations

### `https://sandbox.securegateway.com/`

- Status: `200`
- Title: `Secure Gateway® Sandbox for Penetration Testing-ALSCO`
- Server header: `Secure Gateway®`
- Security headers observed:
  - `X-Frame-Options: SAMEORIGIN`
  - `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload;`
- Links of interest:
  - `/admin/`
  - `/up/`
  - HackerOne program links

### `https://sandbox-royal.securegateway.com/`

- Status: `200`
- Title: `Royal CMS`
- Server header: `Secure Gateway®`
- `Set-Cookie: PHPSESSID=...; path=/`
- Security headers observed:
  - `X-Frame-Options: SAMEORIGIN`
  - `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload;`
- Public endpoints/links observed:
  - `?search`
  - `?tender`
  - `?topic`
  - `?album`
  - `?video`
  - `?block`
  - `?ads`
  - `?lang=ar`
  - `?complaint`
  - `?contact`
  - `?article=8` through `?article=13` from homepage links
  - AJAX endpoint referenced by page JS: `home/program/program-search-fetch.php`

## Low-risk probes performed

### Royal CMS search reflection / JS context

POST `https://sandbox-royal.securegateway.com/?search` with `search=INFO5995MARKER`:

- Status: `200`
- Marker reflected into inline JavaScript:

```js
var userInput = 'INFO5995MARKER';
```

Direct AJAX endpoint `POST /home/program/program-search-fetch.php` with `userInput=INFO5995MARKER`:

- Status: `200`
- Marker reflected in HTML result text:

```html
<span> - <i><b> INFO5995MARKER </b></i> - </span>
```

Sanitization/WAF checks:

- Single quote payload was transformed to backtick-like output in JS/HTML contexts, not a working string breakout.
- Backslash payload was escaped in JS string.
- Obvious `<svg/onload=...>`, `</script`, and similar HTML/JS payloads were blocked by Secure Gateway error page.
- Newline payload is reflected inside the inline JS string, producing a syntactically broken script context, but by itself does not demonstrate script execution.

Current assessment: interesting reflected input handling, but not a confirmed XSS/zero-day yet.

### Royal CMS parameter / exposed file checks

Small manual checks only:

- `?article=13'`, `?article=13"`, `?lang=en'` returned `403` / Secure Gateway block.
- `?article=13 AND 1=2` returned a Secure Gateway page rather than SQL output.
- `robots.txt` returned bot disallow entries, no sensitive path list.
- `.git/HEAD` and config-like paths were blocked/hidden by Secure Gateway (`503`/error page), not exposed.
- `backup.zip`, `uploads/`, `upload/`, `files/` returned `404` or block/error page.
- `/admin/` and `/admin.php` redirect/display Secure Gateway auth page; no open redirect confirmed via `auth/index.php?url=...` quick checks.

## Result

No confirmed zero-day from the ALSCO quick pass yet.

Best next step for ALSCO would require a test account / mobile app path for `sandbox.securegateway.com` 2FA/upload testing, but that may be more setup than “simple”. For quick zero-day hunting, continue target selection rather than spending too long here.
