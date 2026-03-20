# CLAUDE.md — Pool Bracket Project Briefing

This file briefs Claude on the full context of this project so it can make accurate updates without needing the original conversation.

-----

## What This Project Is

A live NCAA Tournament pool tracker for a group of friends. Each player was assigned teams. The bracket tracks:

- First round results and spreads
- Team **transfers** (when a favorite doesn’t cover the spread, their team transfers to whoever owns the underdog)
- Second round matchups as they become available
- Live score updates via the ESPN public scoreboard API

Hosted on GitHub Pages as a single `index.html` file — no build step, no npm, no framework.

-----

## Pool Rules

1. **Favorite covers the spread** → owner keeps the team, team advances
1. **Favorite does NOT cover the spread** → team transfers to the underdog owner (even if the favorite wins the game)
1. **Underdog wins outright** → underdog owner’s team advances
1. **Favorite loses outright** → favorite is eliminated; underdog advances to the underdog owner

Transfer example: Louisville was Dean’s team (-6.5 favorite). Louisville won 83-79 but only covered by 4. Since they didn’t cover -6.5, Louisville transferred to Paul (who owned South Florida, the underdog).

-----

## Players and Their Original Teams

|Player   |Teams (original)                                                                                                 |
|---------|-----------------------------------------------------------------------------------------------------------------|
|Christian|Arkansas, Georgia, Furman, Lehigh (elim’d → got Prairie View A&M)                                                |
|Paul     |South Florida (elim’d), North Carolina (elim’d), Tennessee, Gonzaga (transferred to Lance) → picked up Louisville|
|David    |UConn, VCU, Queens, Howard (elim’d) → picked up Michigan                                                         |
|Travis   |St. John’s, Florida, Arizona, Alabama                                                                            |
|Payton   |Kansas, Iowa, LIU, Virginia                                                                                      |
|Colby    |Siena (elim’d), McNeese (elim’d), Villanova, Hofstra → picked up Duke                                            |
|Gordon   |UCLA, Clemson, Utah State, Akron                                                                                 |
|Nick     |Cal Baptist, Penn (elim’d), Wisconsin (elim’d), Wright State                                                     |
|Quinn    |TCU, Nebraska, High Point, Texas Tech                                                                            |
|Danny    |Ohio State (elim’d), Texas A&M, Hawai’i (elim’d), Miami (OH)                                                     |
|LeGrand  |BYU (elim’d), Tennessee State                                                                                    |
|Wade     |UCF, Troy (elim’d), Texas (NC St/Texas First Four winner), Saint Louis                                           |
|Lance    |N. Dakota St. (elim’d), Illinois, Kennesaw State (elim’d), Kentucky → picked up Gonzaga                          |
|Matt S   |Duke (transferred to Colby), Houston, Miami (FL), Michigan (transferred to David)                                |
|Dean     |Louisville (transferred to Paul), Saint Mary’s (elim’d), Missouri, Santa Clara                                   |
|Vaine    |Northern Iowa, Idaho (elim’d), Purdue, Iowa State                                                                |
|LeGrand  |BYU (elim’d), Tennessee State, Vanderbilt, Nebraska (wait — Quinn has Nebraska)                                  |
|Legrande |BYU (elim’d), Tennessee State                                                                                    |
|Matt S   |Houston, Miami (FL)                                                                                              |


> ⚠️ Note: LeGrand and Legrande are the same person (typo corrected). Matt S and Shurte are the same person (Shurte was a typo for Matt S).

-----

## Confirmed First Round Results (Thu Mar 19)

|Matchup                   |Score |Spread    |Pool Result                                                     |
|--------------------------|------|----------|----------------------------------------------------------------|
|Ohio State vs TCU         |64-66 |OSU -2.5  |🎉 Quinn’s TCU upset win                                         |
|Nebraska vs Troy          |76-47 |NEB -13.5 |✅ Quinn’s Nebraska covered (+29)                                |
|Louisville vs S. Florida  |83-79 |LOU -6.5  |⇌ Transferred to Paul (Dean didn’t cover)                       |
|High Point vs Wisconsin   |83-82 |WIS -9.5  |🎉 Quinn’s High Point upset win                                  |
|Duke vs Siena             |71-65 |DUKE -27.5|⇌ Transferred to Colby (Matt S didn’t cover)                    |
|Vanderbilt vs McNeese     |78-68 |VAN -11.5 |✅ LeGrand’s Vanderbilt covered (+10)                            |
|Arkansas vs Hawai’i       |97-78 |ARK -15.5 |✅ Christian’s Arkansas covered (+19)                            |
|Michigan State vs ND State|92-67 |MSU -16.5 |✅ LeGrand’s Michigan State covered (+25)                        |
|Michigan vs Howard        |101-80|MICH -30.5|⇌ Transferred to David (Matt S didn’t cover — only +21 vs -30.5)|
|UNC vs VCU                |78-82 |UNC -2.5  |🎉 David’s VCU upset win — Paul’s UNC eliminated                 |
|Saint Mary’s vs Texas A&M |50-63 |SMC -3.5  |🎉 Danny’s Texas A&M upset win — Dean’s Saint Mary’s eliminated  |
|BYU vs Texas              |71-79 |BYU -1.5  |🎉 Wade’s Texas upset win — Legrande’s BYU eliminated            |
|Illinois vs Penn          |105-70|ILL -25.5 |✅ Lance’s Illinois covered (+35)                                |
|Georgia vs Saint Louis    |77-102|UGA -2.5  |🎉 Wade’s Saint Louis upset win — Christian’s Georgia eliminated |
|Houston vs Idaho          |78-47 |HOU -22.5 |✅ Matt S’s Houston covered (+31)                                |
|Gonzaga vs Kennesaw State |73-64 |GONZ -18.5|⇌ Transferred to Lance (Paul didn’t cover — only +9 vs -18.5)   |

## Second Round Matchups (Sat Mar 21 — confirmed so far)

|Time CDT|Matchup                     |Pool stakes       |
|--------|----------------------------|------------------|
|11:10 AM|Michigan vs Saint Louis     |David vs Wade     |
|1:45 PM |Michigan State vs Louisville|LeGrand vs Paul   |
|4:15 PM |Duke vs TCU                 |Colby vs Quinn    |
|5:10 PM |Houston vs Texas A&M        |Matt S vs Danny   |
|6:10 PM |Gonzaga vs Texas            |Lance vs Wade     |
|6:50 PM |VCU vs Illinois             |David vs Lance    |
|7:45 PM |Vanderbilt vs Nebraska      |LeGrand vs Quinn  |
|8:45 PM |Arkansas vs High Point      |Christian vs Quinn|

-----

## Live Score Updates

The app uses the **ESPN public scoreboard API** (no key required):

```
https://site.api.espn.com/apis/site/v2/sports/basketball/mens-college-basketball/scoreboard?limit=100
```

This API is called directly from the browser (not server-side), so CORS is not an issue. It returns all current games with scores, status, and clock. The app auto-refreshes every 60 seconds and has a manual refresh button.

### How to match teams from ESPN data

ESPN returns `competitors` with `team.abbreviation` and `team.displayName`. Match against known abbreviations for each pool team. The game `status.type.name` will be:

- `STATUS_SCHEDULED` — not started
- `STATUS_IN_PROGRESS` — live
- `STATUS_FINAL` — complete

-----

## Key Design Decisions

- **Single HTML file** — no build step, works on GitHub Pages with zero config
- **Vanilla JS + inline CSS** — no dependencies, loads fast
- **Bracket layout** — R1 games on left, connector lines, R2 on right within each region
- **Color coding by player** — each player has a unique color pill
- **Transfer logic** — when displayed, shows both original owner and new owner with 🆕 badge
- **Responsive** — 1 column portrait, 2 columns landscape

-----

## How to Update Scores

When asked to update scores:

1. Fetch the ESPN API (or ask the user for scores if working offline)
1. Check each completed game against the spreads above
1. Apply the pool rules (cover/no cover/upset) to determine transfers or eliminations
1. Update the relevant `TeamSlot` data in `index.html`
1. Update the R2 matchup cards with the correct advancing teams
1. Update the timestamp at the bottom

-----

## File Structure

```
ncaa-crazy-pool/
├── index.html      ← The entire app. Edit this to update scores/results.
├── CLAUDE.md       ← This file. Read before making any changes.
└── README.md       ← Setup instructions for GitHub Pages.
```

-----

## GitHub Pages Setup

Once pushed to GitHub:

1. Go to repo **Settings → Pages**
1. Source: **Deploy from branch**
1. Branch: `main`, folder: `/ (root)`
1. Save — site will be live at `https://cmolica.github.io/ncaa-crazy-pool`
