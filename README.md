# Q3-Q4 2026 Trading Dashboard

An interactive, single-file goal tracker for a funded-trading rebuild. Tap goals to check them off, log payouts month by month, and watch progress roll up automatically. Progress saves in your browser, so it persists between visits.

Live site: _add your Vercel URL here after deploying_

---

## What this is

A self-contained web dashboard that turns the Q3-Q4 2026 goal list into a live tracker. It is built on the zero-to-500K rebuild framework: 1% risk per trade, one eval at a time, discipline over speed.

The whole app is one file, `index.html`. There is no build step, no framework, and no server. HTML, CSS, and JavaScript all live in that single file, which is why it deploys to any static host in seconds.

## Concept

The dashboard is organized around four goal categories and a dedicated payout tracker.

**Four categories**
- Funding & Accounts: the eval and account-acquisition goals (5ers, FTMO 200K, Bootcamp, FundedNext, 10x Apex, the 500K rebuild).
- Payouts & Income: the 50K payout goal and Apex payout milestone.
- Obligations & Investments: Eli & Sarah payout, Deriv investment, school fees.
- Strategy & Discipline: goal reviews, strategy re-evaluation, daily journaling.

**Payout tracker**
The 50K target is split across September, October, November, and December. Tap a month when the payout lands and the progress bar and Payouts Banked figure update.

**Rollups**
The header shows three things that recalculate on every tap: an overall completion ring (all goals plus all four payouts), total Payouts Banked, and Obligations Cleared (adds up the dollar-tagged goals as you check them: FTMO $1,200, Eli & Sarah $2,750, Deriv $5,000, school fees $15,000).

## How saving works

Progress is stored in your browser using `localStorage` under the key `q3q4_dashboard_state_v1`. This means:

- Your checkmarks persist when you close and reopen the site.
- Saving is per-browser and per-device. Progress on your laptop does not sync to your phone. Each device keeps its own copy.
- Clearing your browser data, or using a different browser, starts fresh.
- The Reset button wipes all progress on the current device.

There is no account and no database. Nothing leaves your browser. That keeps your financial targets private, but it also means the data is not backed up anywhere. If persistence across devices matters later, that would require adding a backend, which is a larger change.

## Running it locally

Just open `index.html` in any browser (double-click it, or drag it into a browser window). It works fully offline. Saving works locally too.

## Deploying

This repo is set up for GitHub-to-Vercel deployment.

1. Push this folder to a GitHub repo (keep it private, it holds financial targets).
2. Go to vercel.com/new and import the repo.
3. Framework Preset: Other. Leave build and output settings empty. A plain HTML file needs no build.
4. Click Deploy. You get a live URL in about 30 seconds.

After the first deploy, every `git push` triggers an automatic redeploy. No re-import needed.

```bash
git add .
git commit -m "Update goals"
git push
```

## How to edit it in the future

Everything is inside `index.html`. Open it in VS Code. The parts you are most likely to change are near the bottom, inside the `<script>` block.

### Add, remove, or rename a goal

Find the `CATEGORIES` array. Each category has a `goals` list. A single goal looks like this:

```js
{id:"g3", t:"Buy FTMO 200K Account", d:"Use available discounts.", amt:"$1,200", k:"cost", oblig:1200}
```

Fields:
- `id`: a unique tag. Every goal needs its own. If you add one, give it a new id like `g17`. Do not reuse an existing id or the two goals will share the same checkmark.
- `t`: the goal title (the bold line).
- `d`: the short description under it.
- `amt`: the pill on the right (e.g. `"$50K"`, `"Eval"`, `"Daily"`).
- `k`: the pill color. Use `"pos"` for green, `"cost"` for gold, or `""` for plain grey.
- `oblig`: optional. A dollar number that feeds the Obligations Cleared total when the goal is checked. Only add it to real spending or payout obligations. Leave it out otherwise.

To add a goal, copy a line, change the fields, and give it a fresh `id`. To remove one, delete its line. To rename, edit `t` or `d`.

### Change a category name or icon

In the same `CATEGORIES` array, each block has a `name` and an `icon` (an emoji). Edit those directly.

### Change the payout months or amounts

Find the `PAYOUTS` array:

```js
const PAYOUTS = [
  {id:"sep", mo:"September", amt:12500},
  ...
];
```

Edit `mo` for the label and `amt` for the dollar figure. If you change the number of payouts or their total, also update the `payGoal` value inside the `render` function (currently `const payGoal = 50000;`) so the progress bar scales correctly. The "$50,000 goal" label in the HTML and the `$50K` figure in the header would need updating too if the total changes.

### Change colors

At the very top of the file, inside `:root`, there is a list of CSS color variables (`--green`, `--blue`, `--gold`, `--purple`, and so on). Change a hex value there and it updates everywhere that color is used.

### A safety note on editing

The goal data uses a compact JavaScript object format. When editing, keep the commas, quotes, and braces intact. A missing comma or quote will stop the script from running and the dashboard will show the "Loading your progress..." message and go no further. If that happens, open the browser console (F12) to see the error, or undo your last edit.

### If you change goal ids

Saved progress is keyed by goal `id`. If you rename an `id`, any previously checked state for the old id is orphaned (it stays in storage but no longer maps to a visible goal). This is harmless, but if you want a clean slate after big edits, use the Reset button.

## File structure

```
q3-q4-dashboard/
  index.html     the entire app
  README.md      this file
```

That is the whole project. One file does the work; this README explains it.

---

_Not financial advice. Trading involves substantial risk of loss._
