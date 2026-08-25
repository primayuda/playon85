# Session resume — playon85

Use this file to continue work in a new Cursor chat. It contains project context only — no credentials, tokens, or runner personal data.

## Start here — paste into a new Cursor chat

```
Continue the Playon ITB 85 runner roster dashboard.
Read .cursor/SESSION.md and index.html for context.
Repo: github.com/primayuda/playon85
Live: primayuda.github.io/playon85
```

Prior chat: [TS column, Strava, README screenshots](a683995c-67f1-497b-a0a6-1919038ddd73)

## Project

Runner roster dashboard for **Playon ITB 85** at **ITB Ultra Marathon 2026** (16–18 Oktober 2026). Single-page static app that reads registration data from public Google Sheets and groups **runners** by race leg (**R16**, **R8**) plus a **Team Support (TS)** column from a separate sheet.

## Live site & repo

- **GitHub Pages:** https://primayuda.github.io/playon85/
- **GitHub repo:** https://github.com/primayuda/playon85
- **Branch:** `main` (GitHub Pages deploys from repo root)

## Project layout

```
index.html               # Entire app (HTML, CSS, JS)
README.md                # Project overview, live link, screenshots
docs/screenshot-desktop.png
docs/screenshot-mobile.png
favicon.png
.cursor/SESSION.md       # This resume file
```

**Local only (not in repo):** `scripts/` and `reports/` for Strava normalization tooling; listed in `.git/info/exclude`.

## Stack & data flow

- Static HTML/CSS/JS — no build step
- Data: Google Sheets JSON API (`gviz/tq`) via JSONP — two sheet IDs in `index.html`
- Sheets must stay shared as **“Anyone with the link can view”**
- `loadData()` fetches both sheets in parallel (`Promise.all` + `fetchSheet()`)
- Auto-refresh every 5 minutes + manual Refresh button
- If TS sheet fails, runner roster still loads; TS column stays empty until next successful refresh

## Google Sheets

| Sheet | ID | Purpose |
|-------|-----|---------|
| Runner roster | `1TKYamPbDz65cm0HLCMN1obGsUsGF6L8cbWCdxbM4-rA` | R16 / R8 runners |
| Team Support | `1eQIoh0uS6ljakp6ws7AW_JJPUYUfybGOs3EMc5VF6KY` | TS supporters |

### Runner sheet columns (0-indexed)

0 Timestamp, 1 Nama, 2 Jurusan, 3 Kota, 4 HP (not displayed), 5 Kategori (R16/R8/R4), 6–7 …, 8 Strava, …, 10 Transport, 11 Catatan

Runners are bucketed into **R16** and **R8** only (`parseCategories` on col 5). **R4 bucket removed** from dashboard.

### TS sheet columns (0-indexed)

0 Timestamp, 1 Nama, 2 Jurusan, 3 Kota, 4 Nomer HP (not displayed), 5 Support legs/segments, 6 Bentuk Support, 7 Kendaraan, 8 Mensupport bersama siapa, 9 Catatan

## Features implemented

1. **Header** — Eyebrow: “Playon - ITB Ultra Marathon - 16 - 18 Oktober 2026”; title “Runner roster”; stat pills: unique runners, R16, R8, **TS**
2. **Runner cards** — Bib `01` format; R8 sage green; **needs-support** runners get red accent (`.bib.needs-support`)
3. **Team Support column** — Replaces former R4 column; clay/amber styling; `buildTsCard()` shows name, major, city, legs (3-line clamp), support type badges, vehicle, with-who, notes
4. **Mobile** (≤860px) — 2×2 stat grid; tap **R16 / R8 / TS** pills to switch single-column view; column headers hidden
5. **Strava links** on runner cards:
   - Full athlete URLs (`/athletes/{id}`) and `strava.app.link` → **green** (`.strava-valid`, `--sage-dark`)
   - Search links, usernames-only, missing, `Lupa` / `Takada` → **red** (`.strava-invalid` / `.strava-invalid-text`, `--orange-dark`)
   - `isStravaValid()` classifies client-side; no Strava API calls
   - Sheet `HYPERLINK(...)` formulas parsed via `getCell()`
6. **Phone numbers** — not shown on dashboard (runners or TS)
7. **README** — Updated for R16/R8/TS; viewport screenshots from live site:
   - Desktop: `docs/screenshot-desktop.png` (1440×900, three columns incl. Team Support)
   - Mobile: `docs/screenshot-mobile.png` (iPhone 13 profile); shown in README at `width="320"` via HTML `<img>` to avoid full-width blow-up

## Strava data (sheet col 8)

- **All 22 runners** updated manually to canonical `https://www.strava.com/athletes/{id}` URLs (verified via local normalization script + HTTP spot-check, Aug 2026).
- **Strava API validation not implemented** — user chose manual sheet cleanup instead of GitHub Action / OAuth pipeline.
- Re-check locally: `node scripts/report-strava-normalization.js` (writes `reports/strava-normalization.md`).

## Key JS symbols (`index.html`)

- `RUNNER_SHEET_ID`, `TS_SHEET_ID`, `allRunners`, `allTs`, `boardSections = ['R16','R8','TS']`
- `parseRunners()`, `parseTs()`, `buildBibCard()`, `buildTsCard()`, `fetchSheet()`, `render()`
- `stravaUrl()`, `isStravaValid()`, `stravaHtml()`, `stravaLabel()`
- Transport filter applies to R16/R8 only; search applies to runners and TS by name/city

## Recent commits

| Commit   | Summary |
|----------|---------|
| `dc4ce35` | Update session resume with README screenshot work |
| `c87fa68` | Constrain README mobile screenshot to 320px width |
| `0850001` | iPhone 13 viewport for README mobile screenshot |
| `3b356d6` | Fix README desktop screenshot (TS not R4) |
| `5891c5b` | Update README and screenshots for R16/R8/TS layout |
| `b0a0bc0` | Replace R4 column with Team Support from second sheet |

## Safe to share / publish

- Public project URLs, implementation notes
- Sheet IDs already in committed `index.html` (view-only forms)

## Not stored here (on purpose)

- GitHub or Google credentials
- Strava client secret, access tokens, refresh tokens
- Runner names or registration PII
- API keys or `.env` files
- Local `scripts/` / `reports/` normalization tooling

## Possible next steps

- Custom domain for GitHub Pages
- Column index changes if Google Sheet layout changes (see `parseRunners` / `parseTs`)
- Re-capture README screenshots after major UI changes (Playwright + live site URL)
- Re-run local Strava normalization if sheet Strava column changes
