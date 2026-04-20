# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

PokéCRM is a Pokemon card trading business management tool. It is a **single-file, zero-dependency HTML application** — no build step, no framework, no server. Open in any browser directly.

There are two files:
- **`pokecrm.html`** — The main, full-featured version. Use this as the source of truth for all future development.
- **`pokemon_crm_mvp.html`** — An earlier, simpler prototype. Keep for reference; do not actively develop.

## Architecture

Everything lives in a single HTML file: CSS → HTML → JavaScript, in that order.

**State** is a plain JS object at the top of the `<script>` block:
```js
let state = { inventory: [], watchlist: [], grading: [], sales: [], leads: [] };
```
All data is in-memory — there is no backend or localStorage persistence. Data resets on page refresh.

**Roles & Auth** (`pokecrm.html` only): Simulated login (no real auth). Two roles:
- `admin` — full access to all pages
- `staff` — limited to Watchlist search and lead submission

**Pages** are hidden `<div class="page">` elements toggled by `navTo(id, el)`. The active page gets class `active`.

**Month selector** in the topbar controls `currentMonth`/`currentYear`, which filter dashboard metrics and the sales table.

**Key render functions** (called whenever state changes):
- `updateDashboard()` — recalculates all dashboard metrics, charts, donut
- `renderInventory(search?)` — filters and renders the inventory table
- `renderWatchlist()` — renders watchlist table; also shows staff-facing lead cards
- `renderGrading()` — renders grading submissions table
- `updateSalesPage()` — renders sales table and month metrics

**Excel export** (`pokecrm.html` only): Uses the `xlsx` library loaded from CDN (`cdn.jsdelivr.net`). Export buttons call `exportXLSX()`.

**Grading status flow**: `vorbereitung → versendet → bei_psa → zurueck`

## Conventions

- UI language is **German** (labels, placeholders, status text).
- Currency formatting: `fmt(n)` returns `€1.234` style (German locale, no decimals).
- Profit/margin: green (`#5DCAA5 / .profit-pos`) for positive, red (`#F0997B / .profit-neg`) for negative.
- Color palette: dark background `#0a0a0a`, panels `#161820`, accent purple `#7F77DD`.
- All CSS is inline in the `<style>` tag — no external stylesheets.

## Git & GitHub

Repository: https://github.com/achimfeldsmann1-byte/Pokekom

After every meaningful change: commit with a clear message and push to `main`.
