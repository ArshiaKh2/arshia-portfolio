# Security Audit — Arshia Portfolio

**Scope:** public static portfolio repository and client-side code. No destructive testing, exploitation, credential attacks, third-party scanning, or persistence techniques were used.

## Executive summary

No critical/high exploit path was substantiated in the reviewed static portfolio. The production page has no login, server-side API, database, payment flow, user-submitted form, or URL-parameter-driven HTML rendering.

## Control status

### IMPLEMENTED
- **Attack-surface reduction:** production `index.html` no longer depends on Google Fonts or a runtime third-party JavaScript library.
- **Runtime surface:** obsolete presentation variants and unused CSS/JS experiment files were removed; `index.html` is the single production entry point.
- **Privacy minimization:** public contact surface contains professional email and GitHub only. Phone, Telegram and Instagram were removed from the audited build.
- **XSS/DOM hygiene:** repository searches found no `innerHTML` or `eval`; current code does not parse URL parameters or insert user-controlled HTML.
- **External-link hardening:** new-tab external links use `rel="noopener noreferrer"`.
- **CSP baseline:** document-level CSP restricts resource/network origins to self, disables objects, limits forms to self, and does not allow remote images/fonts. Because the page remains a single self-contained HTML file, inline CSS/JS still require `'unsafe-inline'`.
- **Referrer policy:** `strict-origin-when-cross-origin` is declared.
- **Positioning:** public identity is unified as **DIGITAL ENGINEER — WEB · AI · CREATIVE TECHNOLOGY · SECURITY**.
- **No fake security UI:** no fictional SOC/terminal/security-lab surface is presented as a real control.

### TESTED
- Repository code searches returned no matches for: `innerHTML`, `eval`, `api_key`, `token`, `password`, `sk-`, `ghp_`, `AIza`, `BEGIN PRIVATE KEY`.
- Commit-message search for `secret` returned no matching commits in the repository search used for this review.
- Current repository tree was inspected directly.
- `robots.txt`, `sitemap.xml`, `404.html`, and `.well-known/security.txt` were reviewed.
- Current application architecture was reviewed for authentication, backend API, database, payment, forms, and client-side untrusted-input sinks.

**Limitations:** code-search and commit-message searches are not proof that every object across the entire Git history is secret-free. A complete historical secret scan should use a dedicated scanner over every reachable commit when available. The live public URL could not be conclusively fetched from this environment because the external fetch returned a cache miss.

### CONCEPT / DEPLOYMENT
- **Strict CSP without `'unsafe-inline'`:** requires moving inline CSS/JS to same-origin files. Not currently marked implemented.
- **HTTP response headers:** HSTS, `X-Content-Type-Options`, `Permissions-Policy`, and `frame-ancestors` are not claimed as deployed. Static GitHub Pages HTML cannot be assumed to configure arbitrary response headers. Verify the live response or configure them at a supported edge/CDN layer.
- **Server-side validation:** not applicable today because there is no server-side form/API/auth flow. If added later, client validation must never be the security boundary.
- **Branch protection:** `main` should ideally require CI/status checks for stronger repository governance.

## Contact-link verification

The audited build intentionally exposes only email and GitHub. GitHub is the repository identity; email is the supplied professional contact. Telegram and Instagram were not kept in the audited public build because independent destination verification was not available in this review.

## Future application-security rule

For any future form, API, authentication or dashboard:

- validate and authorize on the server;
- treat all browser-provided values as untrusted;
- use parameterized database queries;
- apply CSRF protection where applicable;
- keep secrets in server-side secret storage, never frontend bundles;
- log security events without storing credentials or unnecessary personal data.

## 404 note

`index.html` exists on `main`; repository-side `.nojekyll` and `404.html` are present. A published root 404 should therefore be investigated as a Pages source/configuration, deployment, project-path or cache/propagation issue rather than a missing entry file.
