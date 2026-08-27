# Security Audit — Arshia Portfolio

Scope: public static portfolio repository and client-side code. No destructive testing, exploitation, credential attacks, third-party scanning, or persistence techniques were used.

## Executive summary

No critical/high exploit path was substantiated in the reviewed static portfolio. The current app has no login, server-side API, database, payment flow, user-submitted form, or URL-parameter-driven HTML rendering.

## Control status

### IMPLEMENTED
- **Attack-surface reduction:** removed the Google Fonts runtime dependency from the production `index.html`; no runtime third-party JavaScript library or API is required.
- **Privacy minimization:** public contact surface reduced to professional email and GitHub only. Phone, Telegram, and Instagram were removed from the audited build because those extra destinations were not independently verified in this review.
- **XSS/DOM hygiene:** repository code search found no `innerHTML` or `eval` usage. Current page does not read URL parameters or insert user-controlled HTML into the DOM.
- **External-link hardening:** external links using a new browsing context use `rel="noopener noreferrer"`.
- **CSP baseline:** a same-origin allowlist is present via CSP meta tag. `object-src 'none'`, `base-uri 'self'`, `form-action 'self'`, self-only network/resource directives, and no remote image/font origins are enforced at document level.
- **Referrer policy:** `strict-origin-when-cross-origin` is declared in the document.
- **Positioning:** the public narrative is unified as **DIGITAL ENGINEER — WEB · AI · CREATIVE TECHNOLOGY · SECURITY**.
- **No fake security UI:** there is no fabricated SOC/terminal/security-lab surface presented as a real control.

### TESTED
- GitHub repository code search returned no matches for: `innerHTML`, `eval`, `api_key`, `token`, `password`, `sk-`, `ghp_`, `AIza`, `BEGIN PRIVATE KEY`.
- Commit search for `secret` returned no matching commits in the repository search used for this review.
- Current production tree was inspected and confirmed to be static HTML/CSS/JS with no authentication, backend API, database, or payment workflow.
- `robots.txt`, `sitemap.xml`, `404.html`, and `.well-known/security.txt` were inspected for obvious leakage or configuration problems.

**Limitation:** repository code search and commit-message search are not a cryptographic proof that every object in the entire Git history is secret-free. A complete historical secret scan should be performed with a dedicated tool against every reachable commit when that capability is available.

### CONCEPT / DEPLOYMENT
- **Strict CSP without `unsafe-inline`:** not yet implemented. The current single-file architecture still contains inline CSS/JS, so the CSP baseline intentionally permits inline execution. The next hardening step is moving CSS/JS into same-origin files and removing `unsafe-inline`.
- **HTTP response headers:** HSTS, `X-Content-Type-Options`, `Permissions-Policy`, and `frame-ancestors` are not marked as deployed here. GitHub Pages static content cannot be assumed to set arbitrary response headers from HTML. Verify the live response or place the site behind a header-configurable deployment layer.
- **Server-side validation:** not applicable today because there is no server-side form/API/auth flow. If one is added, client validation must never be treated as the security boundary.
- **Dependency vulnerability scan:** there is no package manifest in this static production surface, but the repository still contains historical/experimental files. Those should not be treated as part of the production runtime.
- **Branch protection:** `main` is currently not protected and has no required status checks. Enabling branch protection/required CI is recommended for engineering hardening.

## Findings and remediation

### M1 — Inline CSS/JavaScript
**Risk:** Medium

Inline code prevents a strict CSP from being used today.

**Remediation:** move CSS and JS to same-origin files, then remove `unsafe-inline` from CSP.

### M2 — Historical experimental files
**Risk:** Low

The repository contains old presentation variants and support files such as `final.html`, `v2.html`, `cinematic-*`, `premium.css`, `three-d.css`, `upgrade.css`, `world.*`, `style.css`, and `script.js`. They are not required by the current single-file production page.

**Remediation:** archive or delete confirmed-obsolete experiments to reduce maintenance and accidental-deploy surface.

### M3 — Response-level security headers
**Risk:** Low/Medium

Document metadata cannot be treated as equivalent to HTTP response headers for all controls.

**Remediation:** verify the published response headers and configure them at a supported edge/CDN/deployment layer when needed.

## Future application security rule

When forms, APIs, authentication, dashboards, or external integrations are introduced:

- validate and authorize on the server;
- treat all client-side values as untrusted;
- use parameterized database queries;
- apply CSRF protection where applicable;
- keep secrets in server-side secret storage, never frontend bundles;
- log security events without storing credentials or unnecessary personal data.

## 404 note

`index.html` exists on `main`. A published root-level 404 is therefore more consistent with Pages source/configuration, deployment state, project path, or cache/propagation than a missing entry file.

## Validation note

The live public URL could not be conclusively fetched from this environment because the external fetch returned a cache miss. Repository-side checks above were performed directly against the GitHub repository.
