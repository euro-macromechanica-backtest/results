# Euro Macromechanica (EMM) M5 Engine — Core Baseline (2008–2025-08) — Retail Standard (7 USD/lot, risk 1%) – Compounding EoY-SoY Base 100k

<p align="center">Balance Curve — Compounding EoY-SoY Base 100k Mode (Risk 1%, $7 round-turn per standard lot, M5 EMM cost model v1.0) 2008–2025-08</p>

<p align="center"><img src="../../../../../docs/assets/2008-2025-08_7usd_balance_curve_comp.png" alt="EMM M5 — Retail Standard — Core Baseline 7 USD/lot — Balance Curve" width="100%"></p>

## 🧾 Track Description

This track reports backtest results for the M5 EMM strategy under **Retail Standard** transaction costs: **7 USD per round‑turn per 1 standard lot (100 000 EUR)**, equivalent to **≈0.7 pips** on EURUSD, with a **dynamic cost model (spread & slippage) M5 EMM cost model v1.0**. Capitalization mode — **compounding across the entire period (EoY→SoY)**. Balance carries over from year to year. **Ending balance → initial balance** of the next year. Initial balance at the start of the period — 100k. Per‑trade risk — **1% of balance at entry**.

- Data range: **Core Baseline 2008-01 – 2025-08** (coverage: **212 months without gaps = 17 years 8 months**)
- Instrument/TF: **EURUSD**, signal logic on **M5**
- **Backtest time zone:** **UTC+0** (all timestamps in UTC+0)
- Cost model: commission, spread, and slippage **included** in PnL

---

## 📈 Year-End Balance `compounding_eoy_soy_base_100k`

| Year | balance at year-end (UTC+0) | year-end percentage (rounded to 5 decimals) |
|---|---:|---:|
| 2008 | 113344.92927 | +13.34493% |
| 2009 | 132836.92159 | +17.19706% |
| 2010 | 170575.35354 | +28.40960% |
| 2011 | 182806.87692 | +7.17074% |
| 2012 | 181935.05837 | −0.47691% |
| 2013 | 186470.59131 | +2.49294% |
| 2014 | 186982.61706 | +0.27459% |
| 2015 | 224044.43206 | +19.82099% |
| 2016 | 249611.32361 | +11.41153% |
| 2017 | 263383.03927 | +5.51726% |
| 2018 | 269627.27566 | +2.37078% |
| 2019 | 271825.38302 | +0.81524% |
| 2020 | 304119.24618 | +11.88037% |
| 2021 | 307503.93907 | +1.11295% |
| 2022 | 325265.77161 | +5.77613% |
| 2023 | 315291.92070 | −3.06637% |
| 2024 | 321554.92921 | +1.98642% |
| 2025-08 | 328969.76186 | +2.30593% |

### Result over 17 years 8 months ~ +228969.76 USD / +228.97%

---

## 🧾 Cost Model

- **Commission:** 7 USD per round‑turn per 1 standard lot (100k EUR)  
- **Cost model (commission, spread, slippage) M5 EMM cost model v1.0** — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv).
- All costs are **included** in PnL.

> Details of the dynamic cost model are provided in [`Euro Macromechanica (EMM) Backtest — Overview and Methodology`](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)

---

## 📊 Summary — Retail Standard 7 USD/lot, `compounding_eoy_soy_base_100k`, risk 1%

### Full period summary 
- **CAGR 6.97%** with annual volatility **5.49%**; risk/return — **Sharpe 1.26**, **Sortino 2.45**, **MAR (Full period Calmar) 1.34**.
- Drawdowns (on the continuous curve): **EoM MaxDD -5.20%**, TTR — **2 months**; deeper intramonth (**-9.12%**), TTR — **2 months**. Time underwater (max episode length): **EoM 26 months**, **Intramonth 22 months**.
- Monthly premium: mean/median month **0.58% / 0.31%**.
- Calendar stability: best year **2010 (28.41%)**, worst **2023 (-3.07%)**; “zero” months **38**.
- Sample size: coverage **2008‑01—2025‑08** (**17 years 8 months**, **212 months**); number of trades: **1443**.
- Additional metrics: share of months “underwater”: **53.30%**; time since MaxDD trough (as of 2025-08) (EoM, months): **202**; time since MaxDD trough (as of 2025-08) (Intramonth, months): **201**; VaR95 (monthly): **-1.35%**; ES95 (monthly): **-2.23%**; VaR99 (monthly): **-2.50%**; ES99 (monthly): **-3.05%**; Downside deviation (annual): **2.82%**; Tail ratio (P95/P5): **2.23**; Omega(0%/month): **3.45**; Gain-to-Pain (monthly): **3.45**; Skewness: **1.65**; Kurtosis excess: **5.89**; Newey–West t/p for mean monthly return: **t=4.43 / p=0.0000**.
- Stress markers: **EoM MaxDD ≈ -5.20%**, **Intramonth MaxDD ≈ -9.12%**; expectation benchmark — **average month ≈ 0.58%**.
> **Summary:** a tidy compounding profile: moderate volatility, limited drawdowns, and a stable monthly premium; resilience is supported by the frequency of positive periods and disciplined risk management.

### Trades summary
- Sample size: **1443** trades; win rate **73.87%**.
- Profile quality: **Profit factor 1.42**, **Payoff 0.50** (avg win/|avg loss|).
- Per‑trade expectancy: **mean 0.08 R**, **median 0.34 R**.
- R‑distribution: σ **0.55 R**, min **-1.03 R**, max **0.59 R**.
- Average outcomes: **avg win 0.38 R**, **avg loss -0.76 R**;
- Worst streaks (sum of R): 5‑tr **-3.77 R**, 10‑tr **-4.72 R**, 20‑tr **-5.75 R**.
- 100‑trade run (EDR): **P50 -3.96 R**, **P95 -2.14 R**.
- Probabilities (per **100 trades**): Pr(MaxDD ≤ −5R) **26.84%**, ≤ −7R **7.34%**, ≤ −10R **0.84%**.
- Max losing streak in 100 trades: **P50 3**, **P95 5**.
- Trade duration: mean **18.00m**, median **13.00m**, P95 **54.00m**, wins **13.00m**, losses **32.00m**.
> **Summary:** the strategy relies on frequent small gains versus rarer, larger losses, so stability rests on stop discipline and constant per‑trade risk. Streak metrics show manageable block drawdowns and indicate that slumps, when they occur, cluster over short spans, while a typical block of trades on average lifts equity. A right‑skewed outcome distribution and contained tail scenarios support predictability; the holding‑period profile matches careful execution without reliance on rare spikes.

### Yearly summary
- Calendar coverage: **2008–2025-08** (year **2025** is partial).
- Calendar‑year return: mean/median **7.13% / 4.00%**; P10/P90 **0.05% / 17.99%**; best/worst **2010 (28.41%) / 2023 (-3.07%)**; years positive **16**, negative **2**.
- Drawdowns (within the year, from peak): **EoM median -1.41%**, range **-5.20% → 0.00%**; **Intramonth median -3.11%**, range **-9.12% → -0.23%**.
- Trading activity: total trades **1443**; win rate (mean/median) **71.90% / 72.77%**; PF (mean/median) **1.81 / 1.44**.
- “Active” by year (means/medians): share of active months **81.02% / 91.67%**; return of actives **7.13% / 4.00%**; active volatility **4.72% / 4.38%**.
- Tail risks by month (avg): **VaR95 -1.03% / ES95 -1.38%**.
> **Summary:** on a yearly horizon the profile remains steadily positive; year‑level drawdowns are compact; “active” periods pull results with restrained volatility; Sharpe/Sortino/Calmar stay solid, and monthly tail risks are limited.

### Monthly returns 
- Coverage: **212** months. Mean/median month: **0.58% / 0.31%** (P10/P90: **-0.96% / 2.15%**).
- Symmetry: positive months **123**, negative **51**, zero **38**.
- Extremes: best month **2010-05 (8.43%)**, worst month **2008-09 (-3.92%)**.
- Runs by month: maximum winning streak — **12** in a row, maximum losing streak — **3** in a row; zero months interrupt any streaks.
> **Summary:** monthly dynamics are compact: the premium is small but repeatable; positives outnumber negatives; prolonged losing streaks are rare; the worst month stays within moderate bounds.

### DD quantiles 
> Quantiles of DD are shown signed (negative), while xRisk = |DD| is published as a positive magnitude. Therefore, as the percentile rises, DD values move toward 0, and xRisk values decrease.
- Observations: **113**; drawdown episodes: **24**.
- Drawdown depth quantiles (EoM, calendar): **P90 -0.29%**, **P95 -0.21%**, **P99 -0.03%**.
- “Underwater” duration: **P90 14 months**, **P95 19 months**.
- Depth in xRisk scale: **P90 0.29**, **P95 0.21**, **P99 0.03**.
> **Summary:** extreme drawdowns at P90–P99 are compact; underwater periods are limited; in the xRisk scale, typical “tail” drawdowns remain moderate relative to the accepted risk.

### Rolling 12m 
- Windows: **201**; incomplete windows: **0**.
- Window return (12m): mean/median **7.48% / 4.61%** (P10/P90: **-0.42% / 20.20%**); best/worst window end: **2011-01 (29.80%) / 2013-02 (-3.71%)**.
- Share of windows by sign: positive **177**, negative **24**, zero **0**.
- Risk/quality (window medians): volatility (annual) **4.16%**, Sharpe **1.53**, Sortino **1.95**, Calmar **4.70**; window MaxDD **-1.28%**.
- Window composition (medians): active **91.67%** (~11 of 12), positive **58.33%**, negative **25.00%**.
- Tails and asymmetry (medians): **Tail 2.38**, **Omega 3.37**; **VaR95 -0.77% / ES95 -1.08%**.
> **Summary:** most yearly windows are positive; 12‑month risk is moderate; within‑window drawdowns are compact; tail risks are small and risk–return is stable.

### Rolling 36m
- Windows: **177**; incomplete windows: **0**.
- Annualized window return: mean/median **6.80% / 5.33%** (P10/P90: **1.43% / 13.77%**); best/worst window end: **2011-10 (22.06%) / 2014-10 (0.51%)**.
- Shares by sign: positive **177**, negative **0**, zero **0**.
- Risk/quality (window medians): volatility (annual) **3.91%**, Sharpe **1.33**, Sortino **3.05**, Calmar **2.86**; window MaxDD **-2.51%**.
- Composition (medians): active **86.11%** (~31 of 36), positive **58.33%**, negative **22.22%**.
- Tails and asymmetry (medians): **Tail 2.79**, **Omega 4.96**; **VaR95 -1.03% / ES95 -1.77%**.
> **Summary:** on the three‑year horizon, windows are consistently positive; risk and within‑window drawdowns are moderate; tails are compact and risk–return remains robust.

### Monte Carlo
- Method: **stationary_bootstrap**.
- Horizons: **12, 36, 212 months**.
- Average block lengths: **3, 4, 5, 6, 7, 8, 9, 10, 11, 12 months**.
- Risk per trade: **1%**.
> **Summary:** across simulations and horizons the picture is clear: on short windows, outcomes are noisier and a small negative finish is possible, yet drawdowns remain workable; on the medium horizon, the distribution compresses, the probability of finishing negative drops sharply, and the profile becomes more predictable; on the long horizon, the vast majority of paths finish decisively positive, sensitivity to the start date falls, and the route features moderate drawdowns in depth and duration. In aggregate, Monte Carlo confirms strategy resilience: longer horizons increase stability of outcomes and reduce the role of randomness while drawdowns remain within expected bounds.

### Confidence Intervals 
- Interval method: **bootstrap_bca** (BCa — bias‑corrected & accelerated).
- Bootstrap (EoM monthly): **stationary_bootstrap**, average block length **6 months**.
- Bootstrap (intramonth): **stationary_bootstrap**, average block length **5 days**.
- Confidence level: **90%**.
> **Summary:** confidence intervals indicate that key estimates are “tight,” without dispersion — intervals are narrow for return and volatility, and the annual premium estimate sits confidently above zero, suggesting the observed effect is not random noise. Intervals for EoM drawdowns are compact and within working bounds, while intramonth intervals are wider and deeper (as expected) yet still without signs of extreme tails. Monthly VaR/ES show compressed tails: rare bad months are bounded and do not push estimates to extremes. Overall, CIs confirm a resilient profile: risk and premium are reproducible on the historical sample, while uncertainty around core metrics is low (with the caveat that intervals describe past distributions and do not account for regime shifts).

### Conclusion
Over 17 years and 8 months, compounding shows steady, “pulling” capital growth at moderate volatility with manageable drawdowns: month‑end dips stay in single‑digit percentages and recover quickly; intramonth swings are deeper but controlled. The monthly premium is small yet repeatable: positive months materially outnumber negative ones; there are no long losing streaks — an outcome of a trade profile with “frequent small gains under strictly limited losses” and disciplined execution. Rolling windows confirm resilience: most yearly intervals are positive; on a three‑year horizon — all; tail risks are compact, and depth/duration of underwater periods are limited. Monte Carlo across horizons adds confidence: short‑window results are noisier, but with horizon length the likelihood of a positive outcome rises sharply and dispersion compresses. Confidence intervals around key metrics are narrow and keep the premium above zero, pointing to reproducibility. In aggregate — even as position size and costs scale with equity — the profile remains resilient and scalable: most of the outcome is driven by the frequency of small positives under controlled risk, not by rare “jackpots”.

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
