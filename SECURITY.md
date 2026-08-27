# Security Audit — Arshia Portfolio

Scope: static portfolio repository and its client-side code. No destructive tests, exploitation, credential attacks, scanning of third-party systems, or persistence techniques were used.

## Executive summary

No critical or high-severity exploit path was identified in the reviewed static portfolio. There is no login system, server-side API, database, payment flow, user-submitted form, or obvious untrusted HTML sink in the current page.

The main findings are hardening and maintenance issues:

### M1 — Inline CSS/JavaScript weakens CSP
**Risk:** Medium  
**Impact:** A strict Content-Security-Policy cannot be applied without allowing inline code. If a future change introduces an HTML injection point, the current inline execution model increases the blast radius.  
**Remediation:** Move executable JavaScript into external files and progressively move styles to external CSS. Use a restrictive CSP with `script-src 'self'`.

### M2 — Third-party Google Fonts dependency
**Risk:** Medium  
**Impact:** The page depends on an external origin for typography. An outage affects appearance, and the browser contacts a third party during page load. It also expands the CSP allowlist.  
**Remediation:** Self-host production fonts in the repository when practical; otherwise explicitly allow only the required font origins.

### M3 — Missing response-level security headers
**Risk:** Low/Medium  
**Impact:** Controls such as HSTS, `X-Content-Type-Options`, and `frame-ancestors` are strongest as HTTP response headers. A static GitHub Pages file cannot reliably configure arbitrary response headers by adding HTML alone.  
**Remediation:** Use platform-supported header configuration where available, or put the site behind a platform/CDN that allows custom security headers.

### M4 — Multiple experimental/duplicate presentation files
**Risk:** Low  
**Impact:** Extra `v2`, `final`, `three-d`, `premium`, and other experimental files increase maintenance and deployment confusion. This is not an active exploit, but it raises the chance of shipping an unintended version.  
**Remediation:** Keep one production entry point and archive/remove obsolete experiments after confirming they are no longer needed.

### L1 — External-link hardening
**Risk:** Low  
**Impact:** Links that open a new browsing context can be exposed to reverse-tabnabbing if `noopener` is omitted.  
**Remediation:** Add `rel="noopener noreferrer"` to every external link using `target="_blank"`.

### L2 — Public contact information
**Risk:** Informational  
**Impact:** The phone number and email are intentionally public contact channels; they can attract spam or scraping.  
**Remediation:** Keep them only if public contact is intended; otherwise use a dedicated professional contact channel.

## 404 finding

`index.html` is present on the `main` branch, so a root-level GitHub Pages 404 is not explained by a missing entry file in the repository. A 404 at the published URL is more consistent with GitHub Pages source/configuration, deployment state, propagation/cache, or an incorrect Pages project path. A repository-side `.nojekyll` and custom `404.html` are useful hardening measures, but they cannot override an incorrect Pages source configuration.

## Validation notes

The live public URL could not be conclusively validated from this environment because the external page fetch returned a cache miss. The repository itself was directly inspected.
