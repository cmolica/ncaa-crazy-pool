# 🏀 NCAA Pool Bracket Tracker

Live bracket tracker for a March Madness pool. Tracks team ownership, point spread coverage, team transfers, and live scores.

## Live Site

👉 [View the bracket](https://cmolica.github.io/ncaa-crazy-pool) *(update this URL after setup)*

## Features

- 📊 First round results with spread outcomes
- ⇌ Automatic transfer tracking (when favorites don’t cover)
- 🏀 Live scores via ESPN public API — auto-refreshes every 60 seconds
- 🗂️ Bracket layout showing R1 → R2 progression with connector lines
- 📱 Responsive — 1 column on mobile, 2 columns on desktop

## Setup (GitHub Pages)

1. Fork or clone this repo
1. Go to **Settings → Pages**
1. Source: **Deploy from branch** → `main` branch, `/ (root)` folder
1. Hit **Save**
1. Your site will be live at `https://cmolica.github.io/ncaa-crazy-pool`

That’s it — no build step, no npm install, no deployment pipeline needed.

## Updating Scores

Open `index.html` and update the game result data. See `CLAUDE.md` for full context on pool rules, player rosters, and how the transfer logic works.

If using Claude Code, just open the project and ask Claude to update the scores — it will read `CLAUDE.md` automatically for full context.
