# Session resume — playon85

Use this file to continue work in a new Cursor chat. It contains project context only — no credentials, tokens, or runner personal data.

## Start here — paste into a new Cursor chat

```
Continue the Playon ITB 85 runner roster dashboard.
Read .cursor/SESSION.md and index.html for context.
Repo: github.com/primayuda/playon85
Live: primayuda.github.io/playon85
```

Prior chat: [TS column and Strava validation](a683995c-67f1-497b-a0a6-1919038ddd73)

## Project

Runner roster dashboard for **Playon ITB 85** at **ITB Ultra Marathon 2026** (16–18 Oktober 2026). Single-page static app that reads registration data from public Google Sheets and groups **runners** by race leg (**R16**, **R8**) plus a **Team Support (TS)** column from a separate sheet.

## Live site & repo

- **GitHub Pages:** https://primayuda.github.io/playon85/
- **GitHub repo:** https://github.com/primayuda/playon85
- **Branch:** `main` (GitHub Pages deploys from repo root)

## Project layout

```
index.html          # Entire app (HTML, CSS, JS)
README.md           # Project overview + live link (still mentions R4 — not updated yet)
docs/screenshot.png # Desktop screenshot for README
docs/screenshot-mobile.png
favicon.png
.cursor/SESSION.md  # This resume file
```

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
4. **Mobile** (≤860px) — Tap **R16 / R8 / TS** stat pills to switch columns
5. **Strava links** on runner cards:
   - Full URLs (including `strava.app.link`) open directly
   - Usernames/names link to Strava athlete search
   - Sheet `HYPERLINK(...)` formulas parsed via `getCell()`
   - `Lupa` / `Takada` shown as “not provided”
6. **Phone numbers** — not shown on dashboard (runners or TS)

## Key JS symbols (`index.html`)

- `RUNNER_SHEET_ID`, `TS_SHEET_ID`, `allRunners`, `allTs`, `boardSections = ['R16','R8','TS']`
- `parseRunners()`, `parseTs()`, `buildBibCard()`, `buildTsCard()`, `fetchSheet()`, `render()`
- Transport filter applies to R16/R8 only; search applies to runners and TS by name/city

## Recent commits

| Commit   | Summary |
|----------|---------|
| `b0a0bc0` | Replace R4 column with Team Support from second sheet |
| `e8405e6` | Header total counts unique runners only |
| `5d604fc` | Site title, favicon, header typography |
| `ab1f77e` | Sage R8 leg; red accent for needs-support runners |
| `fca4f34` | Docs: playon85 repo and Pages URLs |
| `84cc3d2` | Remove phone numbers from runner cards |

## In progress / discussed — NOT implemented

### Strava API validation

User asked to connect to Strava API and validate runner Strava IDs. **Not built yet** — blocked on architecture + credentials.

**Why static site alone cannot do it:**
- Strava API requires OAuth app (client ID + secret) and Bearer token on every request
- Secrets must not live in browser; needs server-side proxy or scheduled job

**Runner Strava field is messy** (typical entries): full names, usernames (e.g. `@handle`), `strava.app.link` URLs, direct `/athletes/{id}` URLs, `Lupa`.

**Proposed validation tiers:**
- **Verified** — numeric athlete ID + `GET /api/v3/athletes/{id}` succeeds (optional name fuzzy-match)
- **Resolved** — app link expanded to athlete ID (server-side redirect follow)
- **Unverified** — username/text only (no Strava search API)
- **Invalid** — ID returns 404
- **Missing** — empty / Lupa / Takada

**Recommended approach:** GitHub Action (nightly) + `strava-validation.json` committed or published as artifact; dashboard loads cache. Alternative: Cloudflare Worker for live validation.

**User decisions still needed:**
1. GitHub Action cache vs Cloudflare Worker?
2. Strava app credentials as GitHub secrets (client ID, secret, refresh token)
3. Validation strictness: ID exists only vs also match registration name?

## Safe to share / publish

- Public project URLs, implementation notes
- Sheet IDs already in committed `index.html` (view-only forms)

## Not stored here (on purpose)

- GitHub or Google credentials
- Strava client secret, access tokens, refresh tokens
- Runner names or registration PII
- API keys or `.env` files

## Possible next steps

- Implement Strava validation pipeline (see above)
- Update `README.md` for TS column and remove R4 references; refresh screenshots
- Update `SESSION.md` prior chat link after new sessions
- Custom domain for GitHub Pages
- Column index changes if Google Sheet layout changes (see `parseRunners` / `parseTs`)
