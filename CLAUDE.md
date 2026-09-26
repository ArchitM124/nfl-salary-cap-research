# CLAUDE.md

## Project
Research project testing whether an NFL team's salary-cap allocation across positions, and its concentration on star players, relates to team performance (2013–2025). The full spec is in `PRD.md`, so read it before starting any task.

## Who I am
Archer, a freshman data science major. I'm new to Python (my coursework is in Java). This is a resume project, and **I have to be able to explain every line in an interview.**

## How to work with me
- **Keep it simple.** Use pandas, matplotlib/seaborn, and statsmodels only. No ML, no scikit-learn pipelines, no classes, and no clever one-liners.
- **Explain as you go.** When you write code, add short comments that say *why*, and give me a 2–3 sentence plain-English summary of what the step does.
- **Small steps.** Do one notebook section at a time. Don't write the whole analysis at once.
- **Don't expand scope.** If something isn't in the PRD, ask before adding it.
- **Flag judgment calls.** If a definition or data choice could reasonably go another way, tell me and let me decide.
- When I ask "why," explain the concept. Don't just rewrite the code.

## Stack
- Python 3, Jupyter notebook
- `nflreadpy` for nflverse data (`load_schedules`, `load_contracts`, `load_pbp`)
- `pandas`, `matplotlib`, `seaborn`, `statsmodels`

## Structure
```
PRD.md
CLAUDE.md
README.md
analysis.ipynb        # main notebook, runs top to bottom
data/raw/             # untouched downloads (gitignored if large)
data/processed/       # team_season.csv (one row per team-season)
charts/               # exported PNGs used in README
```

## Locked definitions (don't change without asking)
- Unit: **team-season**, regular season only, 2013–2025.
- Salary: **cap hit as % of that season's cap**, never raw dollars.
- Primary outcome: **point differential per game** (the season went from 16 to 17 games in 2021).
- Concentration: top player's cap hit ÷ position group total (multi-starter groups only).
- Rookie QB flag: 1 if the primary starter is on his rookie contract (years 1–4 only; the 5th-year option year counts as 0).
- DL/EDGE is one group, for both spending share and concentration.
- Dead money is excluded. Shares only count players on the roster that season.
- Washington is always called "Commanders", even for the Redskins-era seasons.
- Regression uses standard errors clustered by team.

## Gotchas
- **Team relocations:** OAK→LV (2020), SD→LAC (2017), STL→LA (2016). Standardize abbreviations before any merge, and check for rows that fail to merge after every merge.
- **Regression:** position shares sum to ~100%, so **always drop one group as the baseline**. Explain coefficients as "moving cap from baseline to X."
- **Multiple comparisons:** many tests are run, so don't call a single p < 0.05 result a finding without noting this.
- **Causation:** never write "spending causes winning." Use "is associated with."
- **Salary data:** verify that nflverse contracts give per-season cap hits by team. If they don't, stop and ask me before scraping anything. OverTheCap's terms must be checked first.

## Style
- Clear variable names (`team_season`, `qb_cap_share`), no single letters.
- Every chart has a title, axis labels with units, and a caption-ready takeaway.
- README and chart text use plain English with no football jargon.
