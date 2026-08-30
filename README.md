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

## What's in v1

| Area | Status |
|---|---|
| Accounts (bank / cash / wallet), balances | ✅ |
| Categories — seeded set, add/edit, ★ favourite, R routine | ✅ |
| Transactions — quick-add expense / income / transfer, edit, delete, undo | ✅ |
| Home — Income / Expenses / Assets / Net worth tiles, routine checklist, recent | ✅ |
| Money — accounts, holdings (invested vs current), fixed assets | ✅ |
| People — loans lent & borrowed, repayments, optional auto-posted transfer | ✅ |
| Reports — category donut, Needs/Wants/Savings, daily bars, category drill | ✅ |
| Settings — theme (Day/Sepia/Dark/Night), month-start day, JSON export/import, wipe | ✅ |
| Offline / installable PWA | ✅ |

## Not yet built (next passes, per the spec)

- Credit-card module: manual statements (total / min / due), 4 cards, payment tracking
- Recurring / routine automation: `Recurring` records, auto-post, pending cards on Home
- Reports: calendar heatmap, 6-month comparison, budget progress bars
- Savings goals (target amount + date)
- Asset types editor + per-holding valuation history (`ValuationEntry`) + buy/sell flows

## Data model

Stores in IndexedDB `leo-finance`: `accounts`, `categories`, `transactions`,
`holdings`, `loans`, `loanEntries`, `fixedAssets`, `recurring`, plus `meta`
(settings, seed flag). Amounts are integer **paise**. Full model in the design spec.

## Backup

Data is per-device. **Settings → Export JSON** often. Import replaces everything.
