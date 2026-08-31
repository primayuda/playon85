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

Runner roster dashboard for **Playon ITB 85** at **ITB Ultra Marathon 2026** (16–18 Oktober 2026). Single-page static app that reads registration data from public Google Sheets and groups **runners** by race leg (**R16**, **R8**, **R4**) plus a **Team Support (TS)** column from a separate sheet.

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

| Sheet | Edit URL | ID |
|-------|----------|-----|
| Runner roster | https://docs.google.com/spreadsheets/d/1TKYamPbDz65cm0HLCMN1obGsUsGF6L8cbWCdxbM4-rA/edit | `1TKYamPbDz65cm0HLCMN1obGsUsGF6L8cbWCdxbM4-rA` |
| Team Support | https://docs.google.com/spreadsheets/d/1eQIoh0uS6ljakp6ws7AW_JJPUYUfybGOs3EMc5VF6KY/edit | `1eQIoh0uS6ljakp6ws7AW_JJPUYUfybGOs3EMc5VF6KY` |

### Runner sheet columns (0-indexed)

0 Timestamp, 1 Nama, 2 Jurusan, 3 Kota, 4 HP (not displayed), 5 Kategori (R16/R8/R4, slash-separated), 6–7 …, 8 Strava, …, 10 Transport, 11 Catatan

**Kategori examples:** `R16`, `R8`, `R8/R16`, `R4`, `R4/R16`

### TS sheet columns (0-indexed)

0 Timestamp, 1 Nama, 2 Jurusan, 3 Kota, 4 Nomer HP (not displayed), 5 Support legs/segments, 6 Bentuk Support, 7 Kendaraan, 8 Mensupport bersama siapa, 9 Catatan

## Dashboard layout & bucketing

**Columns (desktop, left → right):** R16 | R8 | R4 | Team Support

- **4-column grid** at >1100px (max-width 1400px); **2×2** at ≤1100px; **single column** on mobile (≤860px) via stat-pill leg switch
- **Stat pills:** unique runners, R16, R8, R4, TS
- **Multi-leg runners:** assigned to **one column only** via `primaryCategory()` — priority **R4 → R8 → R16**
  - e.g. `R8/R16` → R8 column only; card badge still shows `R8 / R16`
- **Sheet note (Aug 2026):** no rows with `R4` in Kategori yet — R4 column empty until registrations added

## Features implemented

1. **Header** — Eyebrow: “Playon - ITB Ultra Marathon - 16 - 18 Oktober 2026”; title “Runner roster”; stat pills: unique runners, R16, R8, R4, TS
2. **Runner cards** — Bib `01` format; column accent colors (R16 moss, R8 sage, R4 orange); **needs-support** runners get red accent (`.bib.needs-support`)
3. **Team Support column** — Separate TS sheet; clay/amber styling; `buildTsCard()` shows name, major, city, legs (3-line clamp), support type badges, vehicle, with-who, notes
4. **Mobile** (≤860px) — 2×2 stat grid (5 pills wrap); tap leg pills to switch single-column view
5. **Strava links** on runner cards:
   - Full athlete URLs (`/athletes/{id}`) and `strava.app.link` → **green** (`.strava-valid`)
   - Invalid/missing → **red** (`.strava-invalid` / `.strava-invalid-text`)
   - `isStravaValid()` — client-side only; no Strava API
6. **Phone numbers** — not shown on dashboard (runners or TS)
7. **README** — Still describes R16/R8/TS three-column layout; screenshots pre-R4 — **needs refresh** if UI docs should match live site

## Strava data (sheet col 8)

- All runners manually fixed to canonical `https://www.strava.com/athletes/{id}` URLs (Aug 2026); re-verify after sheet changes
- **Strava API validation not implemented** — manual sheet cleanup; local tooling only
- Re-check: `node scripts/report-strava-normalization.js` → `reports/strava-normalization.md`

## Key JS symbols (`index.html`)

- `RUNNER_SHEET_ID`, `TS_SHEET_ID`, `allRunners`, `allTs`, `boardSections = ['R16','R8','R4','TS']`
- `categoryPriority`, `primaryCategory()`, `parseCategories()`, `parseRunners()`, `parseTs()`, `render()`
- `buildBibCard()`, `buildTsCard()`, `fetchSheet()`
- `stravaUrl()`, `isStravaValid()`, `stravaHtml()`, `stravaLabel()`
- Transport filter applies to R16/R8/R4 only; search applies to runners and TS by name/city

## Recent commits

| Commit   | Summary |
|----------|---------|
| `20adf14` | Multi-leg runners: single column by priority R4 → R8 → R16 |
| `d8d3d5c` | Restore R4 column (R16, R8, R4, TS four-column layout) |
| `e7283d9` | Green/red Strava link styling |
| `dc4ce35` | Session resume; README screenshot work |
| `b0a0bc0` | Team Support column from second sheet (replaced old R4-only layout) |

## Safe to share / publish

- Public project URLs, implementation notes
- Sheet IDs and edit URLs (view-only sharing required for dashboard)

## Not stored here (on purpose)

- GitHub or Google credentials
- Strava client secret, access tokens, refresh tokens
- Runner names or registration PII
- Local `scripts/` / `reports/` normalization tooling

## Possible next steps

- Add R4 registrations in runner sheet (Kategori col)
- Update README + re-capture screenshots for four-column layout
- Custom domain for GitHub Pages
- Re-run local Strava normalization after sheet changes
