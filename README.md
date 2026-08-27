# Arshia Khaleghi — Portfolio

A production-focused, static portfolio for Arshia Khaleghi.

## Architecture

The repository intentionally keeps a small production surface:

- `index.html` — single runtime page, self-contained UI, responsive layout, 3D/CSS scene, interactions and case-study content.
- `404.html` — GitHub Pages fallback.
- `robots.txt` — crawler policy.
- `sitemap.xml` — canonical site map.
- `.nojekyll` — prevents Jekyll processing on GitHub Pages.
- `.well-known/security.txt` — security contact/disclosure metadata.
- `SECURITY.md` — defensive security documentation and deployment limitations.

Legacy duplicate HTML/CSS/JS variants were removed so there is one clear production entry point instead of multiple competing versions.

## Design direction

The portfolio combines:

- Web engineering
- AI / Python experimentation
- Creative technology
- Security-minded development
- Lightweight 3D/CSS interaction
- Persian RTL content

The visual system favors cinematic depth and interaction without depending on a heavy 3D framework.

## Run locally

Open `index.html` directly in a modern browser, or serve the repository with any simple static web server.

## GitHub Pages

Deploy the `main` branch from the repository root using GitHub Pages.

## Security notes

The project is static-first and contains no server-side credentials. The client-side security policy is intentionally restrictive, but hosting-level HTTP response headers are controlled by GitHub Pages and cannot all be configured from `index.html` alone.

See `SECURITY.md` for the defensive audit and limitations.

## Quality rules

- No fake clients, metrics, certifications or security claims.
- No secrets in frontend code.
- Prefer native browser APIs over unnecessary dependencies.
- Respect `prefers-reduced-motion`.
- Keep the deployment surface small.
