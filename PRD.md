# PRD: NFL Salary Cap Allocation & Team Performance

**Owner:** Archer · **Status:** Draft · **Target:** ~4 weeks, a few hours/week

## 1. Question
Every NFL team has the same salary budget (the cap). **Does how a team divides that budget across positions, and whether it pays one star or spreads money across a group, relate to how well it performs?**

## 2. Why it matters
The salary cap is a fixed budget split across positions, much like capital split across assets. This project tests how allocation decisions under a hard constraint relate to outcomes. That's a question finance and fintech interviewers already care about.

## 3. Scope
**In scope**
- Team-seasons from 2013–2025 (32 teams × 13 seasons ≈ 416 rows).
- Cap share by position group (spending share).
- Top-player share within position groups (concentration).
- Rookie-contract QB flag (control).
- Point differential (primary), win % (secondary), and points scored/allowed split by side of the ball (supporting).

**Out of scope**
- Machine learning, web apps, and dashboards.
- Coaching, scheme, and play-calling adjustments (listed as a limitation).
- Individual player value models.
- The QB "pay cut" question (too few cases, no clean definition).

## 4. Definitions (lock these before analysis)
| Term | Definition |
|---|---|
| Unit of analysis | One team in one regular season |
| Salary measure | Each player's **cap hit** for that season, as a **% of that season's cap** (never raw dollars, since the cap grows every year) |
| Position groups | QB, RB, WR/TE, OL, DL/EDGE, LB, CB, S, Special teams (finalize the list once, then keep it) |
| Spending share | Group cap hits ÷ team's total cap hits |
| Concentration | Highest-paid player's cap hit ÷ group's total (only for multi-starter groups: WR, OL, EDGE, LB, CB) |
| Rookie QB flag | 1 if the primary starting QB is on his rookie contract |
| Primary outcome | Point differential **per game** (the season went from 16 to 17 games in 2021) |
| Secondary outcome | Win % |
| Side-of-ball outcomes | Points scored/game (vs. offensive spending) and points allowed/game (vs. defensive spending) |

## 5. Data sources
- **Results:** nflverse schedules via `nflreadpy` (`load_schedules`). Use regular season only.
- **Salaries:** nflverse contracts via `nflreadpy` (`load_contracts`). First check whether it gives per-season cap hits by team. If it doesn't, fall back to OverTheCap's positional spending tables and **check their terms of use before collecting any data**.
- **Optional:** EPA per play from nflverse play-by-play, for the side-of-ball analysis.

## 6. Analysis plan
1. **Descriptive:** Show the average spending share by group, how it has changed over time, and the spread across teams.
2. **Correlations:** Build a heatmap of spending share and concentration against point differential per game.
3. **Regression:** Regress point differential/game on the spending shares, **leaving one group out as the baseline** (the shares sum to 100%). Include the rookie QB flag. Interpret each coefficient as "moving cap from the baseline group to this group."
4. **Concentration:** For each multi-starter group, compare top-player share against point differential.
5. **Timing check:** Does this season's allocation predict **next** season's point differential? This partly addresses the objection that winners simply pay their stars.
6. **Side of ball:** Compare offensive shares against points scored and defensive shares against points allowed.

**Headline result = step 3.** Everything else is supporting. With 30+ comparisons, expect some to look significant by chance, so don't oversell single results.

## 7. Deliverables
- `analysis.ipynb`: a clean, runnable notebook from start to finish.
- `README.md`: the question, definitions, data, method, results, and limitations.
- 3 core charts: a correlation heatmap, rookie QB vs. not, and the regression coefficients.
- A one-paragraph conclusion in plain English (no football jargon).
- A resume line: *"Analyzed 13 seasons of NFL salary-cap allocation across 32 teams (Python/pandas) to test whether positional spending and star concentration relate to team performance; found [result]."*

## 8. Milestones
| Week | Goal |
|---|---|
| 1 | Load results and salary data, build the team-season table, and fix team abbreviations |
| 2 | Descriptive stats and the correlation heatmap |
| 3 | Regression, concentration, timing check, and side of ball |
| 4 | README, final charts, and cleanup. Push to GitHub. |

**If behind:** cut position groups or the side-of-ball section. Don't add methods.

## 9. Known limitations (state them in the README)
- The analysis shows association, not causation. Good teams may pay players *because* they won.
- Coaching, scheme, and QB quality beyond contract status are not controlled for.
- Cap hits are distorted by restructures, void years, and dead money.
- Money paid to injured players never reaches the field.
- The sample is small (~416 rows), and team-seasons aren't fully independent because the same franchises repeat.
- Prior work exists (OverTheCap, PFF, FiveThirtyEight). Cite it and state what this project adds, which is the concentration angle.

## 10. Success criteria
- The notebook runs start to finish on a fresh machine.
- Archer can explain every step and every line without notes.
- The question and result can be explained to a non-football fan in two sentences.
