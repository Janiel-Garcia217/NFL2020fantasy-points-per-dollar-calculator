# NFL2020fantasy-points-per-dollar-calculator

Gives the user the top 25 players to pick on week 12 based on how many points they have averaged up to Week 11 per 1000 dollars.

## Data Source
- Historical NFL Fantasy 2020 by Sam Longenbach on Kaggle. Sourced by RotoGuru.
- Used Columns: Name, Team, Pos, Oppt, Week, DKP, Price (DraftKing values only!)
- Spot checked fantasy scores using FantasyData.com. No real discrepancy found (FantasyData used tenths, RotoGuru uses hundredths).

## Methodology
- **Training window:** Only used data up to week 11 to allow a future follow up where I check and see how well calculating for PPD works.
  - Week 11 was chosen because players should have ideally reached mid-season form and would have built in-season team chemistry.
  - **Minimum games rule:** Players need 3+ real games played in Weeks 1–11 to qualify. This avoids small-sample distortion (e.g., a player with 1-2 great games looking artificially elite).
    - 3 games chosen because if a player was just starting to get playing time, or just coming back from an injury, there would have not been enough data to confidently calculate for PPD.
- **Price:** Each player's real, actual Week 12 salary is used (not averaged), since that's the real number a person would see when building a lineup before Week 12. Unlike points, price isn't a stable trait, it resets weekly based on the market, so averaging it would produce a number that never corresponds to any real decision.
- **Missing data:**
  - Made sure to leave out week 12 stats when merging week 12 prices. This was done to avoid data leakage.
  - Blank weeks (bye weeks, injuries, inactive players) are excluded from averaging rather than treated as zero.
  - Players with no listed Week 12 price (injured, inactive, cut, etc.) are excluded from final rankings, since they can't be evaluated for a Week 12 decision.

## Known Limitations
- Does not distinguish a bye week from a genuine absence when applying the 3-game minimum — a player coming off a bye near the cutoff could be unfairly excluded. Flagged as a future improvement.
- Single-platform (DraftKings) and single-season (2020) scope — intentional, to keep the first version focused and finishable.
- A small number of players have negative average value (real, due to turnovers/fumbles in DraftKings scoring) — included in the full dataset but naturally excluded from the Top 25 output shown.
- This model requires a lot of data filtering and can only inform a decision for week 12. In a future model, I will aim to incorporate functions and definitions to make the process repeatable for weeks past 12.

## Output
A ranked Top 25 list of the best value players heading into Week 12, sorted by points per $1,000 of salary.

## How to Run It
1. Requires Python 3 and pandas
2. Open the notebook in Jupyter
3. Run all cells in order — the CSV must be in the same folder

## Future Improvements
- Handle bye weeks separately from genuine inactivity.
- Add a buffer so players that are coming off multi-week injuries are not being accounted for.
  - These players may need time to get back into full form, so I would not be able to reliably calculate for them.
  - Alternatively, I could program those names to appear with a `*`. That way the user knows that this player is a risky choice because of injury.
- Generalize the pipeline to run for any week, not just Week 12.
- Add a validation step comparing predicted value picks against actual Week 12 performance.
        - These players may need time to get back into full form, so I would not be able to reliably calculate for them
        - Alternatively, I could program those names to appear with a *. That way the user knows that this player is a risky choice               because of injury.
    - Generalize the pipeline to run for any week, not just Week 12.
    - Add a validation step comparing predicted value picks against actual Week 12 performance.
