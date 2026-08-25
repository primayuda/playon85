# Playon ITB 85 — Runner Roster Dashboard

Live dashboard for **Playon ITB 85** at the **ITB Ultra Marathon 2026** (16–18 Oktober 2026). The page pulls runner and team-support signups from Google Sheets and displays them in three columns: **R16**, **R8**, and **Team Support (TS)**.

## Live site

**[primayuda.github.io/playon85](https://primayuda.github.io/playon85/)**

Hosted on GitHub Pages from the `main` branch.

### Desktop

![Playon ITB 85 dashboard — R16, R8, and Team Support columns](docs/screenshot-desktop.png)

### Mobile

On narrow screens, tap **R16**, **R8**, or **TS** in the header to switch columns.

![Playon ITB 85 dashboard on mobile](docs/screenshot-mobile.png)

## Features

- **Live roster** — Fetches the latest runner signups from a Google Sheet (view-only)
- **Team Support** — Separate column for TS volunteers (legs, support type, vehicle, notes)
- **Race legs** — Runners grouped into **R16** and **R8** columns
- **Header stats** — Unique runner count plus R16, R8, and TS totals
- **Search & filter** — Search runners and supporters by name or city; filter runners by transport (self-supported / needs support)
- **Auto-refresh** — Updates every 5 minutes, or click **Refresh** anytime
- **Runner cards** — Bib number, name, major, city, Strava link, transport badges, and notes
- **TS cards** — Supporter name, major, city, support legs, badges, vehicle, and who they support with

## Data sources

Two Google Sheets registration forms feed the dashboard. Both must be shared as **“Anyone with the link can view”**:

| Sheet | Used for |
|-------|----------|
| Runner roster | R16 and R8 runner cards |
| Team Support | TS supporter cards |

Phone numbers from either form are not shown on the dashboard.

## Repository

[github.com/primayuda/playon85](https://github.com/primayuda/playon85)
