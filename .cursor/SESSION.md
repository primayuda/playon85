# Session resume — playon-dashboard-2026

Use this file to continue work in a new Cursor chat. It contains project context only — no credentials, tokens, or runner personal data.

## Project

Runner roster dashboard for **Playon ITB 85** at **ITB Ultra Marathon 2026** (16–18 Oktober 2026). Single-page static app that reads registration data from a public Google Sheet and groups runners by race leg (R16, R8, R4).

## Live site & repo

- **GitHub Pages:** https://primayuda.github.io/playon-dashboard-2026/
- **GitHub repo:** https://github.com/primayuda/playon-dashboard-2026
- **Branch:** `main` (GitHub Pages deploys from repo root)

## Project layout

```
index.html          # Entire app (HTML, CSS, JS)
README.md           # Project overview + live link
docs/screenshot.png # Desktop screenshot for README
docs/screenshot-mobile.png
.cursor/SESSION.md  # This resume file
```

## Stack & data flow

- Static HTML/CSS/JS — no build step
- Data: Google Sheets JSON API (`gviz/tq`) — sheet ID is in `index.html`
- Sheet must stay shared as **“Anyone with the link can view”**
- Auto-refresh every 5 minutes + manual Refresh button

## Features implemented (Aug 2026 session)

1. **GitHub repo** — `primayuda/playon-dashboard-2026`, initial commit pushed
2. **README** — features, live site link, screenshot (no local dev instructions)
3. **GitHub Pages** — enabled from `main` / root
4. **Mobile layout** (≤860px):
   - Responsive header, toolbar, cards, footer
   - Touch-friendly inputs (44px min height, 16px font on inputs)
   - Tap **R16 / R8 / R4** stat pills in header to switch legs (one column at a time)
5. **Strava links** on runner cards:
   - Full URLs (including `strava.app.link`) open directly
   - Usernames/names link to Strava athlete search
   - Sheet `HYPERLINK(...)` formulas parsed via `getCell()`
   - `Lupa` / `Takada` shown as “not provided”
6. **Explicitly not added:** tap-to-call on phone numbers

## Recent commits

| Commit   | Summary                                      |
|----------|----------------------------------------------|
| `1b9fa46` | Initial dashboard HTML                       |
| `cf218fb` | README, mobile layout, GitHub Pages, docs    |
| `0227929` | Strava profile links on runner cards         |

## Resume in Cursor

Paste into a new chat:

```
Continue the Playon ITB 85 runner roster dashboard.
Read .cursor/SESSION.md and index.html for context.
Repo: github.com/primayuda/playon-dashboard-2026
Live: primayuda.github.io/playon-dashboard-2026
```

Prior chat transcript (local Cursor history): [Playon dashboard setup](09e202ea-b974-4ef4-b5a3-1f16673b3e1e)

## Safe to share / publish

This file and the repo contain:

- Public project URLs
- Implementation notes
- Sheet ID already present in committed `index.html` (view-only registration form)

## Not stored here (on purpose)

- GitHub or Google credentials
- Runner names, phone numbers, or other registration fields
- API keys, tokens, or `.env` files

## Possible next steps

- Custom domain for GitHub Pages
- Update README screenshot after UI changes
- Column index changes if the Google Sheet layout changes (see `loadData()` row parser)
- Empty R4 leg UX / filters / sorting
