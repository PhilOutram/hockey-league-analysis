# Hockey League Analysis

Scrapes and analyses the 2025/26 West Hockey Men's league season, following
**Isca M4** through 24 match days.

**[See the charts →](https://philoutram.github.io/hockey-league-analysis/)**

Match results are pulled from the England Hockey fixtures API, reshaped into a
per-team record, accumulated into a league table for every match day, and then
visualised four ways.

| | |
| --- | --- |
| Language | Python 3 (Jupyter notebook) |
| Libraries | `requests`, `pandas`, `matplotlib`, `beautifulsoup4` |
| Notebook | [`m4-results-scraper.ipynb`](m4-results-scraper.ipynb) |

## What it produces

- **Form guide** — a win/draw/loss grid for all twelve teams across the season
- **Cumulative points** — points accruing week by week, Isca M4 highlighted
- **League position by week** — the same season as table position rather than points
- **An animated bar chart race** of the league table across the campaign

All four are in [`assets/`](assets), and the derived league table is in
[`data/hockey_league_cumulative.csv`](data/hockey_league_cumulative.csv).

## How it works

1. Generate the season's Saturday match days, minus the known blank weekends.
2. Fetch each match day from the England Hockey competitions API and keep the
   fixtures that have a result.
3. Expand every fixture into two rows, one per team, deriving W/D/L and goal
   difference.
4. Accumulate points and sort by the league's tiebreak rules to get a standings
   snapshot after each match day.
5. Plot the four visualisations from that table.

## Running it

The API needs a key, which is read from the environment rather than committed:

```bash
export EH_API_KEY="your-key"      # Windows: setx EH_API_KEY "your-key"
pip install requests beautifulsoup4 pandas matplotlib ipywidgets
jupyter notebook m4-results-scraper.ipynb
```

Note that the England Hockey endpoint used here (`eh-dw-prod.azurewebsites.net`)
now returns 403 and appears to have been retired, so the scraping cells will not
currently fetch new data. The notebook keeps the outputs from the 2025/26 run, and
the derived table is in `data/` if you want to re-plot without scraping.
