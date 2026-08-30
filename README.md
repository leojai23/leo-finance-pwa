# Leo Finance

A single-user personal money ledger — income, daily expenses, accounts, loans,
and net worth. Static PWA, INR only, works offline, all data stays on the device
in IndexedDB. Same delivery model as the Leo Interview notebook.

**Design spec:** https://claude.ai/code/artifact/e2eca4dd-e75d-49a3-8e86-c7092205f691

## Run it

Open `index.html` over HTTP (a service worker needs a real origin, not `file://`):

```
cd Leo-Finance
python -m http.server 8080
# then visit http://localhost:8080
```

Or just deploy to GitHub Pages and open the URL on the phone → **Add to Home screen**.

## Deploy (GitHub Pages, same as the notebook)

```
git init && git add . && git commit -m "Leo Finance v1"
git branch -M main
git remote add origin https://github.com/leojai23/leo-finance-pwa.git
git push -u origin main
```

Then in the repo: **Settings → Pages → Deploy from branch → `main` / root**.
Live at `https://leojai23.github.io/leo-finance-pwa/`.

When `index.html` changes, bump `CACHE_NAME` in `sw.js` so clients pick it up.

## What's built

| Area | Status |
|---|---|
| Accounts (bank / cash / wallet), balances | ✅ |
| Categories — seeded set, add/edit, ★ favourite, R routine, monthly budget | ✅ |
| Transactions — quick-add expense / income / transfer, edit, delete, undo | ✅ |
| Home — Income / Expenses / Assets / Net-worth tiles, "Due now", routine checklist, recent | ✅ |
| Money — accounts, holdings (invested vs current, valuation history, buy/sell), fixed assets | ✅ |
| Asset types — 7 presets + user-defined types with custom fields (Settings → Asset types) | ✅ |
| Credit cards — spend from card, manual statements (total/min/due), pay-bill flow, in net worth | ✅ |
| People — loans lent & borrowed, repayments, optional auto-posted transfer | ✅ |
| Recurring & routines — monthly/weekly, auto-post on boot or "Due now" Confirm/Skip | ✅ |
| Savings goals — target + date, progress bar, ₹/mo needed | ✅ |
| Reports — donut, Needs/Wants/Savings, budgets, daily bars, calendar heatmap, 6-month compare | ✅ |
| Settings — theme (Day/Sepia/Dark/Night), month-start day, JSON export/import, recurring & category editors | ✅ |
| Offline / installable PWA | ✅ |

## Rough edges / not yet done

- Recurring reminders are in-app only (no notifications — by design for a Pages PWA)
- No multi-currency, bank CSV import, or cloud sync (deferred per the spec)

The full design spec is now implemented.

## Data model

Stores in IndexedDB `leo-finance` (v3): `accounts`, `categories`, `transactions`,
`holdings`, `valuations`, `assetTypes`, `loans`, `loanEntries`, `fixedAssets`,
`recurring`, `creditCards`, `cardStatements`, `goals`, plus `meta` (settings, seed flag).
Amounts are integer **paise**. Full model in the design spec.

## Backup

Data is per-device. **Settings → Export JSON** often. Import replaces everything.
