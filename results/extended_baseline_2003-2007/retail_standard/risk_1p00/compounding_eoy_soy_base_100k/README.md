# Euro Macromechanica (EMM) M5 Engine — Extended Baseline (2003–2007) — Retail Standard (7 USD/lot, risk 1%) – Compounding EoY-SoY Base 100k

<p align="center">Balance Curve — Compounding EoY-SoY Base 100k Mode (Risk 1%, $7 round-turn per standard lot, M5 EMM cost model v1.0) 2003–2007</p>

<p align="center"><img src="../../../../../docs/assets/2003-2007_7usd_balance_curve_comp.png" alt="EMM M5 — Retail Standard — Extended Baseline 7 USD/lot — Balance Curve" width="100%"></p>

## 🧾 Track Description

This track reports backtest results for the M5 EMM strategy under **Retail Standard** transaction costs: **7 USD per round‑turn per 1 standard lot (100 000 EUR)**, equivalent to **≈0.7 pips** on EURUSD, with a **dynamic cost model (spread & slippage) M5 EMM cost model v1.0**. Capitalization mode — **compounding across the entire period (EoY→SoY)**. Balance carries over year to year. **Ending balance → initial balance** of the next year. Initial balance at the start of the period — 100k. Per‑trade risk — **1% of balance at entry**.

- Data range: **Extended Baseline 2003-01 – 2007-12** (coverage: **60 months without gaps = 5 years**)
- Instrument/TF: **EURUSD**, signal logic on **M5**
- **Backtest time zone:** **UTC+0** (all timestamps in UTC+0)
- Cost model: commission, spread, and slippage **included** in PnL

---

## 📈 Year-End Balance `compounding_eoy_soy_base_100k`

| Year | balance at year-end (UTC+0) | year-end percentage (rounded to 5 decimals) |
|---|---:|---:|
| 2003 | 107707.83975 | +7.70784% |
| 2004 | 115385.68506 | +7.12840% |
| 2005 | 110772.96595 | −3.99765% |
| 2006 | 114174.88422 | +3.07107% |
| 2007 | 114365.17970 | +0.16667% |

### Result over 5 years ~ +14365.18 USD / +14.37%

---

## 🧾 Cost Model

- **Commission:** 7 USD per round‑turn per 1 standard lot (100k EUR)  
- **Cost model (commission, spread, slippage) M5 EMM cost model v1.0** — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv).
- All costs are **included** in PnL.

> Details of the dynamic cost model are provided in [`Euro Macromechanica (EMM) Backtest — Overview and Methodology`](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)

---

## 📊 Summary — Retail Standard 7 USD/lot, `compounding_eoy_soy_base_100k`, risk 1%

### Full period summary
- **CAGR 2.72%** with annual volatility **3.79%**; risk–return profile — **Sharpe 0.73**, **Sortino 1.24**, **MAR (Full period Calmar) 0.49**.
- Drawdowns (on the continuous curve): **EoM MaxDD -5.54%**; recovery time — **not recovered (n/a)**; deeper intramonth (**-7.50%**), **TTR — not recovered (n/a)**. Time underwater: **EoM 38 months**, **Intramonth 38 months**.
- Monthly premium: mean/median month **0.23% / 0.21%**.
- Calendar stability: best year **2003 (7.71%)**, worst **2005 (-4.00%)**; “zero” months **4**.
- Sample size: coverage **5** years, **60** months; number of trades: **342**.
- Stress markers: **EoM MaxDD ≈ -5.54%**, **Intramonth MaxDD ≈ -7.50%**; expectation benchmark — **average month ≈ 0.23%**.
> **Summary:** a conservative profile with low volatility, shallow drawdowns, and a stable modest monthly premium; performance is supported by a high share of positive periods given disciplined risk management.

### Trades summary
- Sample size: **342** trades; win rate **69.88%**.
- Profile quality: **Profit factor 1.19**, **Payoff 0.51** (avg win/|avg loss|).
- Per‑trade expectancy: **mean 0.04 R**, **median 0.28 R**.
- R‑distribution: **σ ≈ 0.56 R**, **min -1.03 R**, **max 0.57 R**.
- Average outcomes: **avg win 0.37 R**, **avg loss -0.72 R**.
> **Summary:** positive expected value with a high share of winning trades and payoff below 1 — a “frequent small wins, rarer larger losses” profile; stability is delivered by risk‑management discipline and loss control.

### Yearly summary
- Coverage: **5** years (2003–2007). Mean/median calendar year: **2.82% / 3.07%**.
- Best year: **2003 (7.71%)**; worst year: **2005 (-4.00%)**.
- Drawdowns (within the year, from peak): **EoM -4.71% → -1.08%**, **Intramonth -5.54% → -2.22%**.
- Trading activity: total trades across years **342**; yearly averages — win rate **69.55%**, PF **1.19**.
> **Summary:** on a yearly horizon, the profile remains even: the average calendar result is stable, within‑year drawdowns are contained, and trading metrics confirm the prevalence of frequent, small positive periods.

### Monthly returns
- Coverage: **60** months (2003–2007). Mean/median month: **0.23% / 0.21%** (P10/P90: **-1.09% / 1.64%**).
- Symmetry: positive months **37**, negative **19**, zero **4**.
- Extremes: best month **2004-02 (2.92%)**, worst month **2005-07 (-2.92%)**.
- Runs by month: maximum winning streak — **10** consecutive months, maximum losing streak — **4** consecutive months; months with zero return interrupt any streak.
> **Summary:** a small but steady premium with moderate tails; stability is supported by the high share of positive months and the absence of prolonged losing streaks.

### Conclusion
The track appears steady‑moderate: the equity curve rises evenly with low variability, drawdowns are shallow though recovery of prior peaks can be protracted; the monthly “premium” is small but stable and supported by a high share of positive periods without long losing streaks. By years, dynamics avoid extremes: best and worst stretches stay within comfortable bounds, and in‑year drawdowns remain controlled. Trading statistics confirm the “frequent small wins with modest payoff” model — disciplined risk management and loss control yield a positive expected value suitable for careful capital growth over the full horizon.

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
- Cost model (commission, spread, slippage) M5 EMM cost model v1.0 — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv)
- General information about the contents of `results`: **[results/README.md](https://github.com/euro-macromechanica-backtest/results/blob/main/results/README.md)**
- Inputs and provenance: **[INPUTS-PIN.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INPUTS-PIN.md)** / **[INPUTS-PROVENANCE.md](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md)**
- Full audit guide: **[docs/AUDIT.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/AUDIT.md)**
- Data quality policy: **[data_quality_policy/policy_v1.0.md](https://github.com/euro-macromechanica-backtest/results/blob/main/data_quality_policy/policy_v1.0.md)**
- Metric calculation methodology: **[docs/metrics_methodology/metrics_schema.md](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.md)** / **[docs/metrics_methodology/metrics_schema.json](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.json)**
