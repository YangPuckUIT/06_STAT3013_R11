# Randomness, Pity Mechanisms, and Pricing in Gacha Games

**STAT3013 · Group [YY] · Class [XX]**

Project title: *The Effects of Randomness, Pity Mechanisms, and Pricing on Player Satisfaction and Spending in Gacha Games: Implications for Game Revenue*

This repository holds the analysis code, data and outputs for the group project. The report answers five questions in five stages, using 510 cleaned survey responses and monthly revenue data for Genshin Impact and Honkai: Star Rail.

> **Status:** this repository currently contains **Stage 2 (Inferential)**. Other stages will be added by their owners.

## The five stages

| Stage | Question | Methods | Owner |
|---|---|---|---|
| 1. Descriptive | Who are the players, how much do they spend, is spending concentrated? | Descriptive statistics, Gini, Lorenz curve | Huyền |
| 2. Inferential | Are the links seen in the sample real, or due to chance? | Spearman, chi-square, Holm correction | Phúc |
| 3. Regression | Which factors are associated with higher spending? | Ordered logit | Minh |
| 4. Satisfaction | Do players who spend more also feel more satisfied? | % satisfied with Wilson 95% CI | Anh |
| 5. ML forecasting | Do new-character banners improve the revenue forecast? | ARIMA, LightGBM | Hương |

We write "is associated with", not "causes": the survey is a one-time snapshot.

## Data

- **Survey:** Google Forms, anonymous, adults (18+) who played a gacha game in the past 12 months. Recruitment was through online communities, so the sample is **not random**. The target population is adult Vietnamese gacha players active in online communities.
- **Cleaning (761 → 510 rows):** removed 8 non-players, 104 under-18s, 72 who failed the attention check (Q31), 60 with contradictory answers and 7 who gave the same answer to every question.
- **File:** `Dataset/gacha_clean.csv` (510 rows, 52 columns). Do not edit it by hand or re-save it from Excel.

Variables used in Stage 2:

| Variable | From | Meaning |
|---|---|---|
| LUCK | Q13–15 | How lucky players feel (1–5 average) |
| PITY2 | Q17–18 | How fair the pity system feels (replaces PITY, alpha 0.54) |
| PRICE | Q19–21 | How fair prices feel |
| SAT | Q22–24 | Satisfaction |
| RET1 | Q28 | Plans to keep playing (replaces RET, alpha 0.48) |
| PROMO | Q33 | Spends more during promotion events (Yes/No) |
| SPEND_MONTH4 | Q5 | 0 = nothing, 1 = <100k, 2 = 100–300k, 3 = 300k+ VND per month |

The top monthly spending bands (300k and above) are merged into one group because the separate bands have too few respondents (26, 10 and 15).

## Stage 2 results

Holm correction is applied across all five tests. All five remain significant (adjusted p < 0.001).

| Test | Statistic | 95% CI |
|---|---|---|
| LUCK ↔ SAT (Spearman) | ρ = 0.308 | 0.219 – 0.393 |
| PITY2 ↔ SAT (Spearman) | ρ = 0.443 | 0.366 – 0.515 |
| PRICE ↔ SAT (Spearman) | ρ = 0.454 | 0.382 – 0.524 |
| SAT ↔ RET1 (Spearman) | ρ = 0.504 | 0.434 – 0.570 |
| PROMO × SPEND_MONTH4 (chi-square) | χ²(3) = 144.44, Cramér's V = 0.532 | – |

Sensitivity check, respondents who spend each month (n = 292): χ²(2) = 47.79, Cramér's V = 0.405. The association with promotions does not depend only on the zero-spending group.

Full tables and figure: `outputs/stage2/`.

## Repository structure

```
.
├── Data_Analysis/
│   └── Stage2_Inferential.ipynb   # Stage 2 notebook
├── Dataset/
│   └── gacha_clean.csv            # cleaned survey data (510 rows)
├── outputs/
│   └── stage2/
│       ├── figures/               # PNG figures
│       └── tables/                # CSV result tables
├── requirements.txt
└── README.md
```

## How to run

Requires Python 3.13.

```powershell
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

Open `Data_Analysis/Stage2_Inferential.ipynb` in VS Code, select the `.venv` kernel and choose **Restart and Run All**. The notebook reads `Dataset/gacha_clean.csv` and writes its tables and figure to `outputs/stage2/`.

## Limitations

- Non-random sample recruited online; about 80% of respondents are aged 18–22, and most play Zenless Zone Zero or Genshin Impact.
- Cross-sectional survey: results describe associations, not causes.
- Spending is self-reported in bands; the top bands are merged.
- Intention to keep playing (RET1) is concentrated at the top of the scale, which may lower its correlation with satisfaction.

## Data source and use

Survey collected by the group for the STAT3013 course project. Responses are anonymous. This repository is for academic use.
