<p align="center">Balance Curve — Compounding EoY-SoY Base 100k Mode (Risk 1.5%, $5 round-turn per standard lot, M5 EMM cost model v1.0) 2003–2025-08</p>

<p align="center"><img src="../../../../../docs/assets/2003-2025-08_5usd_balance_curve_comp.png" alt="EMM M5 — Retail Rebate — Composite Baseline 5 USD/lot — Balance Curve" width="100%"></p>

# Euro Macromechanica (EMM) M5 Engine — Composite Baseline (2003–2025-08) — Retail Rebate (5 USD/lot, risk 1.5%) – Compounding EoY-SoY Base 100k

## 🧾 Track Description

This track reports backtest results for the M5 EMM strategy under **Retail Rebate** transaction costs: **5 USD per round‑turn per 1 standard lot (100 000 EUR)**, equivalent to **≈0.5 pips** on EURUSD, with a **dynamic cost model (spread & slippage) M5 EMM cost model v1.0**. Capitalization mode — **compounding across the entire period (EoY→SoY)**. Capital carries over from year to year. **Ending balance → initial balance** of the next year. Initial balance at the start of the period — 100k. Per‑trade risk — **1.5% of balance at entry**.

- Data range: **Composite Baseline 2003-01 – 2025-08** (coverage: **272 months without gaps = 22 years 8 months**)
- Instrument/TF: **EURUSD**, signal logic on **M5**
- **Backtest time zone:** **UTC+0** (all timestamps in UTC+0)
- Cost model: commission, spread, and slippage **included** in PnL

---

## 📈 Year-End Balance `compounding_eoy_soy_base_100k`

| Year | balance at year-end (UTC+0) | year-end percentage (rounded to 5 decimals) |
|---|---:|---:|
| 2003 | 112857.03606 | +12.85704% |
| 2004 | 126465.25193 | +12.05792% |
| 2005 | 119621.50165 | −5.41157% |
| 2006 | 125626.14681 | +5.01970% |
| 2007 | 126135.19998 | +0.40521% |
| 2008 | 154332.59106 | +22.35489% |
| 2009 | 197051.03761 | +27.67947% |
| 2010 | 288369.84478 | +46.34272% |
| 2011 | 323116.99399 | +12.04951% |
| 2012 | 321829.00907 | −0.39861% |
| 2013 | 334672.75517 | +3.99086% |
| 2014 | 336412.99798 | +0.51998% |
| 2015 | 444812.66836 | +32.22220% |
| 2016 | 524943.73053 | +18.01456% |
| 2017 | 568592.87943 | +8.31501% |
| 2018 | 588717.24109 | +3.53933% |
| 2019 | 595879.32993 | +1.21656% |
| 2020 | 700406.21482 | +17.54162% |
| 2021 | 711730.35138 | +1.61680% |
| 2022 | 766232.55264 | +7.65770% |
| 2023 | 729611.64143 | −4.77935% |
| 2024 | 750123.38267 | +2.81132% |
| 2025-08 | 772451.30878 | +2.97657% |

### Result over 22 years 8 months ~ +672451.31 USD / +672.45%

---

## 🧾 Cost Model

- **Commission:** 5 USD per round‑turn per 1 standard lot (100k EUR)  
- **Cost model (commission, spread, slippage) M5 EMM cost model v1.0** — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv).
- All costs are **included** in PnL.

> Details of the dynamic cost model are provided in [`Euro Macromechanica (EMM) Backtest — Overview and Methodology`](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)

---

## 📊 Summary — Retail Rebate 5 USD/lot, `compounding_eoy_soy_base_100k`, risk 1.5%

### Full period summary
- **CAGR 9.44%** with annual volatility **7.86%**; risk–return profile — **Sharpe 1.19**, **Sortino 2.40**, **MAR (Full period Calmar) 1.24**.
- Drawdowns (on the continuous curve): **EoM MaxDD −7.64%**; recovery time — **26 months**; deeper intramonth (**−12.78%**), **TTR — 2 months**. Time underwater: **EoM 39 months**, **Intramonth 42 months**.
- Monthly premium: mean/median month **0.78% / 0.42%**.
- Calendar stability: best year **2010 (46.34%)**, worst **2005 (−5.41%)**; “zero” months **41**.
- Sample size: coverage **22 years 8 months** (2003‑01—2025‑08), **272** months; number of trades: **1785**.
- Stress markers: **EoM MaxDD ≈ −7.64%**, **Intramonth MaxDD ≈ −12.78%**; expectation benchmark — **average month ≈ 0.78%**.
> **Summary:** a stable, steadily rising profile with moderate volatility and limited EoM drawdowns; intramonth swings are deeper but recover quickly. The monthly premium is small yet persistent over the long sample; resilience is driven by the frequency of positive periods and disciplined risk management.

### Trades summary
- Sample size: **1785** trades; win rate **73.17%**.
- Profile quality: **Profit factor 1.39**, **Payoff 0.51** (avg win/|avg loss|).
- Per‑trade expectancy: **mean 0.08 R**, **median 0.33 R**.
- R‑distribution: **σ ≈ 0.55 R**, **min -1.02 R**, **max 0.59 R**.
- Average outcomes: **avg win 0.38 R**, **avg loss -0.74 R**.
> **Summary:** positive expected value with a high share of winning trades and payoff below 1 — a profile of “frequent small wins vs. less frequent but larger losses”; resilience is ensured by strict risk control.

### Yearly summary
- Coverage: **22 years 8 months** (2003‑01—2025‑08; calendar coverage **2003–2025**, 2025 is partial). Mean/median calendar year: **9.94% / 5.02%**.
- Best year: **2010 (46.34%)**; worst year: **2005 (-5.41%)**.
- Drawdowns (within the year, from peak): **EoM -7.12% → 0.00%**, **Intramonth -12.78% → -0.34%**.
- Trading activity: total trades across years **1785**; yearly averages — win rate **71.41%**, PF **1.68**.
> **Summary:** by year, dynamics remain even: a positive average without extremes; within‑year drawdowns are contained. Yearly trading metrics confirm profile resilience.

### Monthly returns
- Coverage: **272** months (2003‑01—2025‑08). Mean/median month: **0.78% / 0.42%** (P10/P90: **−1.55% / 3.21%**).
- Symmetry: positive months **161**, negative **70**, zero **41**.
- Extremes: best month **2010-05 (13.17%)**, worst month **2008-09 (−5.62%)**.
- Runs by month: maximum winning streak — **12** consecutive months, maximum losing streak — **4** consecutive months; months with zero return interrupt any streak.
> **Summary:** a small and stable monthly premium; a high share of positive months without prolonged losing streaks.

### Conclusion
The track is deliberately assembled as a “long showcase” of stability: over 22 years 8 months, with mixed data quality and a fixed risk of ~1.5% per trade, the equity curve trends steadily upward with moderate volatility; EoM drawdowns remain shallow, intramonth swings are more pronounced yet typically recover quickly, and extended periods underwater are limited in depth. The monthly premium is small but repeatable, which is reflected in yearly dynamics without extreme spikes or slumps. The trade profile — high hit rate with payoff below one — relies on frequent small wins under strict loss control, producing positive expectancy and predictable risk–return characteristics even across a “noisy” blend of market regimes. In sum, this demonstrates the intended purpose of the track: stable behavior over a very long interval at a moderately elevated per‑trade risk.

### Full methodology and metric definitions in [`docs/metrics_methodology/metrics_schema.json`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.json) / [`docs/metrics_methodology/metrics_schema.md`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.md).

### Metrics files

```
metrics/
  monthly_returns.csv
  full_period_summary.csv
  yearly_summary.csv
  trades_full_period_summary.csv
```

### Metrics were computed based on non‑public files `trades_YYYY.csv` and `balance_YYYY.csv`

---

## 📎 Links

- **Euro Macromechanica (EMM) Backtest — Overview and Methodology**: repository root **[README.md](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)**
- General information about the contents of `results`: **[results/README.md](https://github.com/euro-macromechanica-backtest/results/blob/main/results/README.md)**
- Cost model (commission, spread, slippage) M5 EMM cost model v1.0 — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv)
- Inputs and provenance: **[INPUTS-PIN.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INPUTS-PIN.md)** / **[INPUTS-PROVENANCE.md](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md)**
- Full audit guide: **[docs/AUDIT.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/AUDIT.md)**
- Data quality policy: **[data_quality_policy/policy_v1.0.md](https://github.com/euro-macromechanica-backtest/results/blob/main/data_quality_policy/policy_v1.0.md)**
- Metric calculation methodology: **[docs/metrics_methodology/metrics_schema.md](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.md)** / **[docs/metrics_methodology/metrics_schema.json](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.json)**
