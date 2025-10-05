# Euro Macromechanica (EMM) M5 Engine — Core Baseline (2008–2025-08) — Retail Rebate (5 USD/lot, risk 1%) – Compounding EoY-SoY Base 100k

<p align="center">Balance Curve — Compounding EoY-SoY Base 100k Mode (Risk 1%, $5 round-turn per standard lot, M5 EMM cost model v1.0) 2008–2025-08</p>

<p align="center"><img src="../../../../../docs/assets/2008-2025-08_5usd_balance_curve_comp.png" alt="EMM M5 — Retail Rebate — Core Baseline 5 USD/lot — Balance Curve" width="100%"></p>

## 🧾 Track Description

This track reports backtest results for the M5 EMM strategy under **Retail Rebate** transaction costs: **5 USD per round‑turn per 1 standard lot (100 000 EUR)**, equivalent to **≈0.5 pips** on EURUSD, with a **dynamic cost model (spread & slippage) M5 EMM cost model v1.0**. Capitalization mode — **compounding over the whole period (EoY→SoY)**. Balance carries over year to year. **Ending balance → initial balance** of the next year. Initial balance at the start of the period — 100k. Per‑trade risk — **1% of balance at entry**.

- Data range: **Core Baseline 2008-01 – 2025-08** (coverage: **212 months without gaps = 17 years 8 months**)
- Instrument/TF: **EURUSD**, signal logic on **M5**
- **Backtest time zone:** **UTC+0** (all timestamps in UTC+0)
- Cost model: commission, spread, and slippage **included** in PnL

---

## 📈 Year-End Balance `compounding_eoy_soy_base_100k`

| Year | balance at year-end (UTC+0) | year-end percentage (rounded to 5 decimals) |
|---|---:|---:|
| 2008 | 115063.86183 | +15.06386% |
| 2009 | 136533.57160 | +18.65895% |
| 2010 | 176953.26852 | +29.60422% |
| 2011 | 191244.62684 | +8.07635% |
| 2012 | 190956.19712 | −0.15082% |
| 2013 | 196116.38538 | +2.70229% |
| 2014 | 196863.80466 | +0.38111% |
| 2015 | 238102.08511 | +20.94762% |
| 2016 | 266287.11152 | +11.83737% |
| 2017 | 281540.69501 | +5.72825% |
| 2018 | 288773.39485 | +2.56897% |
| 2019 | 291249.23200 | +0.85736% |
| 2020 | 327337.35603 | +12.39080% |
| 2021 | 331097.63518 | +1.14875% |
| 2022 | 352963.22850 | +6.60397% |
| 2023 | 342917.96005 | −2.84598% |
| 2024 | 350124.05278 | +2.10140% |
| 2025-08 | 359544.45136 | +2.69059% |

### Result over 17 years 8 months ~ +259 544.45 USD / 259.54%

---

## 🧾 Cost Model

- **Commission:** 5 USD per round‑turn per 1 standard lot (100k EUR)  
- **Cost model (commission, spread, slippage) M5 EMM cost model v1.0** — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv).
- All costs are **included** in PnL.

> Details of the dynamic cost model are provided in [`Euro Macromechanica (EMM) Backtest — Overview and Methodology`](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)

---

## 📊 Summary — Retail Rebate 5 USD/lot, `compounding_eoy_soy_base_100k`, risk 1%

### Full period summary 
- **CAGR 7.51%** with annual volatility **5.59%**; risk/return — **Sharpe 1.33**, **Sortino 2.74**, **MAR (Full period Calmar) 1.66**.
- Drawdowns (on the continuous curve): **EoM MaxDD −4.52%**, **TTR — 2 months**; deeper intramonth (**−8.42%**), **TTR — 1 month**. Time underwater (max episode length): **EoM 26 months**, **Intramonth 22 months**.
- Monthly premium: mean/median month **0.62% / 0.32%**.
- Calendar stability: best year **2010 (29.60%)**, worst **2023 (−2.85%)**; “zero” months **38**.
- Sample size: coverage **2008‑01—2025‑08**, **17 years 8 months**;  **212** months; number of trades: **1443**.
- Additional metrics: share of months “underwater” **51.89%**; time since MaxDD trough: **EoM 202 months / Intramonth 201 months**; **VaR/ES (95%) −1.28% / −2.14%**, **VaR/ES (99%) −2.45% / −2.94%**; **Downside deviation (annual) 2.70%**; **Tail ratio (P95/P5) 2.43**; **Omega(0%/month) 3.80**; **Gain‑to‑Pain (monthly) 3.80**; **Skewness 1.81**; **Kurtosis excess: 6.44**; **Newey–West t/p for mean month: t=4.55 / p=0.00**.
- Stress markers: **EoM MaxDD ≈ −4.52%**, **Intramonth MaxDD ≈ −8.42%**; expectation benchmark — **average month ≈ 0.62%**.
> **Summary:** over the full period the strategy delivers a steady premium at moderate volatility: drawdowns are shallow and rebound quickly, monthly returns are stable with right‑hand skew, and the calendar profile is even — strong years offset weak ones without prolonged slumps; the time spent underwater is modest by risk quality, tails are contained and consistent with VaR/ES intervals and a significant t‑statistic, so the baseline expectation is a small but predictable monthly gain at a comfortable risk level.

### Trades summary
- Sample size: **1443** trades; win rate **74.01%**.
- Profile quality: **Profit factor 1.46**, **Payoff 0.51** (avg win/|avg loss|).
- Expectancy per trade: **mean 0.09 R**, **median 0.34 R**.
- R‑distribution: **σ 0.55 R**, **min −1.02 R**, **max 0.59 R**.
- Averages: **avg win 0.39 R**, **avg loss −0.75 R**.
- Worst streaks (sum of R): **5‑tr −3.74 R**, **10‑tr −4.67 R**, **20‑tr −5.64 R**.
- 100‑trade run (EDR): **P50 −3.87 R**, **P95 −2.10 R**.
- Probability (per **100 trades**): Pr(MaxDD ≤ −5R) **24.62%**, ≤ −7R **6.18%**, ≤ −10R **0.68%**.
- Max losing streak in 100 trades: **P50 3**, **P95 5**.
- Duration: **mean 18.00m**, **median 13.00m**, **P95 54.00m**, **wins 13.00m**, **losses 32.00m**.
> **Summary:** the strategy relies on frequent small gains versus rarer, larger losses, so stability rests on stop discipline and constant per‑trade risk. Streak metrics show manageable block drawdowns and indicate that slumps, when they occur, cluster over short “blocky” segments, while a typical block of trades on average lifts equity. A right‑skewed outcome distribution and contained tail scenarios support predictability, and the holding‑period profile matches careful execution without reliance on rare spikes.

### Yearly summary
- Calendar coverage: **2008–2025-08** (year **2025** is partial).
- Mean/median calendar year: **7.69% / 4.21%**.
- Best/worst year: **2010 (29.60%)**, **2023 (−2.85%)**.
- Drawdowns (within the year, from peak): **EoM −4.52% → 0.00%**, **Intramonth −8.42% → −0.22%**.
- Trading activity: total trades **1443**; yearly averages — win rate **72.30%**, PF **1.87**.
- “Active” metrics by year (averages): share of active months **81.02%**, return of actives **7.69%**, active volatility (annual) **4.77%**.
- Tail risk by month (yearly average): **VaR95 −0.98% / ES95 −1.32%**.
> **Summary:** on the yearly horizon the profile remains steadily positive, calendar drawdowns are compact, “active” periods pull results at moderate volatility; risk‑adjusted quality (Sharpe/Sortino/Calmar) holds, and monthly tail risks are contained.

### Monthly returns 
- Coverage: **212** months. Mean/median month: **0.62% / 0.32%** (P10/P90: **−0.89% / 2.21%**).
- Symmetry: positive months **125**, negative **49**, zero **38**.
- Extremes: best month **2010-05 (8.74%)**, worst month **2008-09 (−3.69%)**.
- Streaks by month: longest winning streak — **12** in a row, longest losing streak — **3** in a row; zero months break streaks.
> **Summary:** monthly dynamics are compact: the premium is small but repeatable; positive months dominate; prolonged losing streaks are rare; the “worst” month remains moderate.

### DD quantiles 
> DD quantiles are shown signed (negative), while xRisk = |DD| is published as a positive magnitude. Therefore, as the percentile rises, DD values approach 0, and xRisk values decrease.
- Observations: **110**; drawdown episodes: **24**.
- Drawdown depth quantiles (EoM, calendar): **P90 −0.28%**, **P95 −0.16%**, **P99 −0.07%**.
- Time underwater: **P90 14 months**, **P95 19 months**.
- Depth in xRisk scale: **P90 0.28**, **P95 0.16**, **P99 0.07**.
> **Summary:** extreme drawdowns by P90–P99 are compact, underwater periods are limited; in xRisk terms, typical “tail” drawdowns remain moderate relative to the chosen risk level.

### Rolling 12m
- Windows: **201**; incomplete windows: **0**.
- Return per window (12m): mean/median **8.04% / 5.01%** (P10/P90: **−0.19% / 21.84%**); best/worst window end: **2009-10 (31.67%) / 2013-02 (−3.46%)**.
- Share of signs: positive **178**, negative **23**, zero **0**.
- Risk/quality (window medians): volatility (annual) **4.21%**, Sharpe **1.61**, Sortino **2.22**, Calmar **5.54**; window MaxDD **−1.25%**.
- Window composition (medians): active **91.67%** (~11 of 12), positive **58.33%**, negative **16.67%**.
- Tails and asymmetry (medians): **Tail 2.57**, **Omega 3.92**; **VaR95 −0.75% / ES95 −1.03%**.
> **Summary:** in 12‑month windows results are stable: the vast majority of windows are positive, volatility is moderate, and within‑window drawdowns are shallow and recover quickly. Risk‑adjusted metrics stay strong, tails are contained, and “green” months dominate — a predictable, comfortable risk profile over a one‑year horizon.

### Rolling 36m 
- Windows: **177**; incomplete windows: **0**.
- Annualized window return: mean/median **7.28% / 5.71%** (P10/P90: **1.75% / 14.63%**); best/worst window end: **2011-10 (23.45%) / 2014-10 (0.76%)**.
- Sign shares: positive **177**, negative **0**, zero **0**.
- Risk/quality (window medians): volatility (annual) **4.06%**, Sharpe **1.35**, Sortino **3.16**, Calmar **3.10**; window MaxDD **−2.48%**.
- Composition (medians): active **86.11%** (~31 of 36), positive **58.33%**, negative **19.44%**.
- Tails and asymmetry (medians): **Tail 2.98**, **Omega 5.71**; **VaR95 −1.00% / ES95 −1.68%**.
> **Summary:** on the 36‑month horizon the profile is steady and predictable: all windows are positive, returns are smooth at moderate volatility; risk‑adjusted metrics are solid, and within‑window drawdowns are shallow and controlled. Active and “green” months dominate; tail risks are restrained — yielding a smooth, risk‑comfortable trajectory over long spans.

### Monte Carlo
- Method: **stationary_bootstrap**.
- Horizons: **12, 36, 212 months**.
- Average block lengths: **3, 4, 5, 6, 7, 8, 9, 10, 11, 12 months**.
- Risk per trade: **1%**.
> **Summary:** On the short horizon, the median outcome is positive with moderate drawdowns and rare breaches of hard thresholds; on the medium horizon, the probability of a negative result becomes minimal, drawdowns remain controlled, and serious breaches are infrequent and require long time; on the full horizon, negative outcomes are virtually excluded, scenario dispersion tightens, tails are restrained, and “no‑breach” of strict thresholds is maintained in most paths. Overall, simulations confirm robustness: the longer the horizon, the more predictable the outcome and the calmer the risk profile.

### Confidence Intervals 
- Interval method: **bootstrap_bca** (BCa — bias‑corrected & accelerated).
- Bootstrap (EoM monthly): **stationary_bootstrap**, average block length **6 months**.
- Bootstrap (intramonth): **stationary_bootstrap**, average block length **5 days**.
- Confidence level: **90%**.
> **Summary:** confidence intervals confirm statistically significant positive returns over the full horizon with moderate uncertainty on volatility. Calendar EoM drawdowns remain relatively shallow and narrow in range, while intramonth drawdowns are deeper as expected yet without signs of “destructive” tails; VaR/ES metrics consistently indicate restrained extremes. At the yearly level the picture is similar: within‑year drawdowns are controlled and the loss distribution remains stable. Overall, intervals are compact, the strategy effect is durable and not driven by single outliers, and the risk profile is even and manageable.  

### Conclusion
Across the 17 years and 8 months horizon the track delivers a resilient positive premium at moderate risk: drawdowns are shallow and recover quickly (both on closes and intramonth), monthly dynamics are smooth with dominance of “green” periods, and yearly/rolling windows confirm a steady trajectory without prolonged slumps; Monte Carlo and confidence intervals consistently show restrained tails and high predictability of outcomes, while trade‑series metrics indicate manageable local drawdowns and a stable contribution from typical trades. With a fixed **$5** commission per trade the overall cost load is lower than the standard scenario, so risk‑return metrics and profile quality improve accordingly.

### Full methodology and metric definitions in [`docs/metrics_methodology/metrics_schema.json`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.json) / [`docs/metrics_methodology/metrics_schema.md`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.md).

### Metrics files

```
metrics/
  confidence_intervals.csv
  dd_quantiles_full_period.csv
  monthly_returns.csv
  monte_carlo_summary.csv
  full_period_summary.csv
  rolling_12m.csv
  rolling_36m.csv
  trades_full_period_summary.csv
  yearly_summary.csv
```

### Metrics were computed based on non‑public files `trades_YYYY.csv` and `balance_YYYY.csv`

---

## 📎 Links

- **Euro Macromechanica (EMM) Backtest — Overview and Methodology**: repository root **[README.md](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)**
- Cost model (commission, spread, slippage) M5 EMM cost model v1.0 — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv)
- General information about the contents of `results`: **[results/README.md](https://github.com/euro-macromechanica-backtest/results/blob/main/results/README.md)**
- Inputs and provenance: **[INPUTS-PIN.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INPUTS-PIN.md)** / **[INPUTS-PROVENANCE.md](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md)**
- Full audit guide: **[docs/AUDIT.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/AUDIT.md)**
- Data quality policy: **[data_quality_policy/policy_v1.0.md](https://github.com/euro-macromechanica-backtest/results/blob/main/data_quality_policy/policy_v1.0.md)**
- Metric calculation methodology: **[docs/metrics_methodology/metrics_schema.md](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.md)** / **[docs/metrics_methodology/metrics_schema.json](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.json)**
