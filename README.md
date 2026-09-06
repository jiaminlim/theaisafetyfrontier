# The AI Safety Frontier

Weekly-cadence AI safety dashboard, organised on the 2026 *International AI Safety Report* taxonomy.
Live at **https://theaisafetyfrontier.com**

- `index.html` — the entire site. Self-contained: inline CSS + JS, no build step, no dependencies.
- `REFRESH.md` — operating manual for the daily refresh (taxonomy, sources, card conventions, validation).
- `CNAME` — custom domain for GitHub Pages.

## How it deploys

GitHub Pages serves `main` from the repo root. Any push to `main` redeploys within a minute.

## How it refreshes

A scheduled Claude agent runs daily: pulls the 12 tracked sources, classifies what it finds against
the rules in `REFRESH.md`, edits `index.html`, validates, and pushes. See `REFRESH.md` §9–§11.
