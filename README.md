# IPL Match & Player Performance Analysis

> **The toss winner won 50.8% of matches. That's a coin flip.**
>
> 1,095 matches. 260,920 balls. 17 seasons. Every captain in the country calls the toss like it decides something.

A Python data-science analysis of every Indian Premier League match from **2008 to 2024**, built
ball-by-ball — cleaning the raw data, merging it into innings-level tables, and finding out what
actually predicts performance in T20 cricket.

---

## 📸 The output

### Does winning the toss matter?

**No.** Across the 1,090 matches with a result, the toss winner won **50.8%** of them — statistically
indistinguishable from flipping a coin. Every pre-match interview, every captain's "we'll bowl first,
it's a chase" reasoning, is defending a coin.

![Impact of the toss](Required%20output/fig7_toss_impact.png)

### Which teams actually win?

Mumbai Indians lead on total wins, but totals mostly measure *longevity* — they've played more seasons.

![Wins by team](Required%20output/fig4_team_wins.png)

### Who's the best batsman?

V Kohli — **8,004 runs** from 6,065 balls, a strike rate of **132.0**.

![Top batsmen](Required%20output/fig5_top_batsmen.png)

### Who takes the most wickets?

YS Chahal — **213 dismissals** on his bowling.

![Top bowlers](Required%20output/fig6_top_bowlers.png)

### Has scoring actually changed?

The distribution of innings scores, and how they move across seasons:

![Distribution of innings runs](Required%20output/fig1_runs_distribution.png)

![Total runs per season](Required%20output/fig3_runs_per_season.png)

### How do wickets get taken?

Caught dominates — around two-thirds of dismissals depend on a fielder taking a catch, not the bowler.

![Dismissal kinds](Required%20output/fig8_dismissals.png)

### The full dashboard

![EDA dashboard](Required%20output/fig9_dashboard.png)

---

## 🔢 Key findings

| # | Finding |
|---|---|
| 1 | A typical innings scores **157 runs** (median 160) for the loss of about 6 wickets |
| 2 | **551 innings** (24.8%) reach 180+ — a total that would have been exceptional a decade ago |
| 3 | Highest innings: **287** (Sunrisers Hyderabad, 2024) |
| 4 | **Toss winner won 50.8%** of matches — the toss is a coin flip |
| 5 | Mumbai Indians have the most wins (**142**); Chennai Super Kings follow |
| 6 | V Kohli leads on runs (**8,014**); YS Chahal on wickets (**213**) |
| 7 | Total runs in the dataset: **347,756** across 2,217 innings |

---

## 🧹 Why the cleaning step matters more than the charts

The raw Kaggle dataset is genuinely messy, and every one of these mistakes would have quietly corrupted
the results:

| Problem | Raw data | Fixed |
|---|---|---|
| Renamed franchises counted as separate teams | 19 team labels | **15 franchises** |
| Season labels split across two years (`2007/08`, `2020/21`) | 17 mixed labels | consistent |
| Blank cities (Dubai, Sharjah) | 51 missing | filled |
| Super-over balls mixed into innings totals | 161 balls, 14 matches | flagged |
| Run outs credited to the bowler | inflates wicket counts | flagged |

The franchise merge is the one that matters most. Without it, **Delhi Daredevils** and **Delhi
Capitals** are two different "teams", so their wins never appear on the same bar — and the same for
Kings XI Punjab / Punjab Kings, Royal Challengers Bangalore / Bengaluru, and Rising Pune
Supergiants / Supergiant. One franchise ends up looking like two mid-table clubs.

```python
TEAM_RENAMES = {
    'Royal Challengers Bangalore': 'Royal Challengers Bengaluru',
    'Delhi Daredevils':           'Delhi Capitals',
    'Kings XI Punjab':            'Punjab Kings',
    'Rising Pune Supergiants':    'Rising Pune Supergiant',
}
for col in ['team1', 'team2', 'toss_winner', 'winner']:
    matches[col] = matches[col].replace(TEAM_RENAMES)
```

---

## 🎯 Why this project exists

This is the **PBL (Project Based Learning) research-writing activity** for
**Python for Data Science (BE05000231)** — BE 3rd Year, Semester 5, Computer Engineering, at
**Sardar Vallabhbhai Patel Institute of Technology, Vasad** (GTU).

The brief was to take a real, large, genuinely dirty dataset and work through the complete
data-science workflow end to end — import, inspect, clean, merge, engineer features, analyse,
visualise, and validate. IPL cricket data fits that perfectly: it's large (a quarter-million balls),
publicly available, and riddled with the exact inconsistencies the syllabus asks you to handle.

**Objective:** answer questions cricket fans actually argue about — does the toss matter, who
really performs, and has the game actually changed — using evidence rather than opinion.

📄 Full academic report: [`Micro Project Report/`](Micro%20Project%20Report/)

---

## 🛠 Tech stack

| Layer | Tool | Used for |
|---|---|---|
| Language | Python 3 | everything |
| Data handling | pandas | `read_csv`, cleaning, `replace`, `merge`, `groupby`, `describe` |
| Numerical | NumPy | calculations |
| Visualisation | Matplotlib | chart sizing, titles, saving PNGs |
| Visualisation | Seaborn | `histplot`, `boxplot`, heatmap |
| Environment | Jupyter Notebook | step-by-step execution with visible output |

---

## 📁 Project structure

```
IPL_Analysis/
├── Complete Source Code/
│   ├── IPL_Analysis.ipynb      # the full analysis, top to bottom
│   ├── matches.csv             # 1,095 matches (tracked)
│   └── deliveries.csv          # 260,920 balls (not tracked - see below)
├── Micro Project Report/
│   └── IPL_Analysis_PBL_Report.pdf
├── Required output/            # all 10 generated charts
│   ├── fig1_runs_distribution.png
│   ├── fig2_runs_boxplot.png
│   ├── fig3_runs_per_season.png
│   ├── fig4_team_wins.png
│   ├── fig5_top_batsmen.png
│   ├── fig6_top_bowlers.png
│   ├── fig7_toss_impact.png
│   ├── fig8_dismissals.png
│   ├── fig9_dashboard.png
│   └── fig10_correlation.png
└── .gitignore
```

---

## 🚀 Running it

```bash
pip install pandas numpy matplotlib seaborn notebook
```

Then drop `matches.csv` and `deliveries.csv` into `Complete Source Code/` and run the notebook.
Paths resolve relative to the notebook, so it works wherever you clone it.

**Dataset:** [IPL Complete Dataset 2008–2024](https://www.kaggle.com/datasets/nowke9/ipldata) (Kaggle).

`deliveries.csv` is ~27 MB and is intentionally left out of the repository to keep the clone fast —
the notebook already has all outputs saved, so you can read every result without running anything.
Download it from Kaggle if you want to re-run the analysis yourself.

---

## ⚠️ Known limitations

Being straight about what this notebook does and doesn't do:

- **Bowler wickets count every dismissal on the ball**, including run outs. Crediting only bowler
  dismissals gives YS Chahal **205** rather than 213.
- **Super-over balls are still included** in innings totals (161 balls across 14 matches).
- **The correlation heatmap is computed at ball level**, not innings level, so it shows
  `batsman_runs ↔ total_runs` at 0.98 rather than the more meaningful innings-level relationships.
- The academic report describes an extended analysis (innings phases, strike rate and economy rates,
  venue normalisation, automated validation checks) that is **documented in the report but not yet
  implemented in the notebook**.

---

## 👥 Team

| Name | Enrolment |
|---|---|
| Bardanwala Husain | 240410107008 |
| Bardanwala Taha | 240410107009 |
| Bhalani Miral | 240410107011 |
| Nikunj Dhobi | 240410107033 |

**Faculty guide:** Prof. Keyur Suthar

---

*Dataset: IPL Complete Dataset 2008–2024, via Kaggle. Analysis and code by the team above.*
